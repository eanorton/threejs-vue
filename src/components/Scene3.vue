<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from 'three';
import { GUI } from 'three/examples/jsm/libs/dat.gui.module';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

let camera, scene, renderer;

export default {
  name: 'Scene3',
  data: () => ({
    params: {
      clipIntersection: true,
      planeConstant: 0,
      showHelpers: false
    },
    clipPlanes: [
      new THREE.Plane( new THREE.Vector3( 1, 0, 0 ), 0 ),
      new THREE.Plane( new THREE.Vector3( 0, - 1, 0 ), 0 ),
      new THREE.Plane( new THREE.Vector3( 0, 0, - 1 ), 0 )
    ],
  }),
  methods: {
    init() {
      const container = document.getElementById('container');
      renderer = new THREE.WebGLRenderer( { antialias: true } );
      renderer.setPixelRatio( window.devicePixelRatio );
      renderer.setSize( window.innerWidth, window.innerHeight );
      renderer.localClippingEnabled = true;
      container.appendChild( renderer.domElement );

      scene = new THREE.Scene();

      camera = new THREE.PerspectiveCamera( 40, window.innerWidth / window.innerHeight, 1, 200 );

      camera.position.set( - 1.5, 2.5, 3.0 );

      let controls = new OrbitControls( camera, renderer.domElement );
      controls.addEventListener( 'change', this.render ); // use only if there is no animation loop
      controls.minDistance = 1;
      controls.maxDistance = 10;
      controls.enablePan = false;

      let light = new THREE.HemisphereLight( 0xffffff, 0x080808, 1.5 );
      light.position.set( - 1.25, 1, 1.25 );
      scene.add( light );

      let group = new THREE.Group();

      for ( let i = 1; i <= 30; i += 2 ) {
        let geometry = new THREE.SphereBufferGeometry( i / 30, 48, 24 );

        let material = new THREE.MeshLambertMaterial( {
          color: new THREE.Color().setHSL( Math.random(), 0.5, 0.5 ),
          side: THREE.DoubleSide,
          clippingPlanes: this.clipPlanes,
          clipIntersection: this.params.clipIntersection
        });
        group.add( new THREE.Mesh( geometry, material ) );
      }
      scene.add( group );

      let helpers = new THREE.Group();
      helpers.add( new THREE.PlaneHelper( this.clipPlanes[ 0 ], 2, 0xff0000 ) );
      helpers.add( new THREE.PlaneHelper( this.clipPlanes[ 1 ], 2, 0x00ff00 ) );
      helpers.add( new THREE.PlaneHelper( this.clipPlanes[ 2 ], 2, 0x0000ff ) );
      helpers.visible = false;
      scene.add( helpers );

      let gui = new GUI();

      gui.add( this.params, 'clipIntersection' ).name( 'clip intersection' ).onChange( function ( value ) {
        let children = group.children;

        for ( let i = 0; i < children.length; i ++ ) {
          children[ i ].material.clipIntersection = value;
        }
        this.render();
      });

      gui.add( this.params, 'planeConstant', - 1, 1 ).step( 0.01 ).name( 'plane constant' ).onChange( function ( value ) {
        for ( let j = 0; j < this.clipPlanes.length; j ++ ) {
          this.clipPlanes[j].constant = value;
        }

        this.render();
      });

      gui.add( this.params, 'showHelpers' ).name( 'show helpers' ).onChange( function ( value ) {
        helpers.visible = value;
        this.render();
      });
      window.addEventListener( 'resize', this.onWindowResize, false );
    },
    onWindowResize() {
			camera.aspect = window.innerWidth / window.innerHeight;
			camera.updateProjectionMatrix();
			renderer.setSize( window.innerWidth, window.innerHeight );
			this.render();
    },
    render() {
			renderer.render( scene, camera );
		},
  },
  mounted() {
    this.init();
    this.render();
  },
};
</script>
