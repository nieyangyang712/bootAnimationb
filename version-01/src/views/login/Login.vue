<template>
  <div class="login-container" style="background-color: #141a48;margin: 0px;overflow: hidden;">
    <div id="canvascontainer" ref="can"></div>
    <div class="firstdemo">
        <el-form ref="form" :model="form" label-width="80px" class="login_form">
        <el-row type="flex" justify="center" class="form_item">
            <el-col :span="5">
            <el-form-item label="用户名">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            </el-col>
        </el-row>
        <el-row type="flex" justify="center" class="form_item">
            <el-col :span="5">
            <el-form-item label="密码">
                <el-input v-model="form.password"></el-input>
            </el-form-item>
            </el-col>
        </el-row>
        <el-row type="flex" justify="center" class="form_item">
            <el-col :span="5">
            <el-form-item>
                <el-button type="primary" @click="onSubmit">登陆</el-button>
            </el-form-item>
            </el-col>
        </el-row>
        </el-form>
    </div>
</div>
</template>
<script>
import * as THREE from "three"
export default {
  name: "fisrtdemo",
  data() {
    return {
      form: { name: "", password: "" },
      token: localStorage.getItem("Authorization")
    };
  },
  methods: {
    onSubmit() {
      if (this.form.name == "admin" && this.form.password == "111111") {
        this.$message({
          message: "登陆成功",
          type: "success"
        });
        localStorage.setItem("Authorization", 111111);
        this.$router.push({ path: "/" });
      } else {
        this.$message.error("登陆失败");
      }
    },
    login() {
      console.log(11);
      if (this.token) {
        // 存储在本地的localStograge中，可以使用cookies/local/sessionStograge
        // this.$store.commit(types.LOGIN, this.token)
        // 跳转至其他页面
        let redirect = decodeURIComponent(this.$route.query.redirect || "/");
        this.$router.push({
          path: redirect
        });
      }
    }
  },
  mounted() {
    this.login();

  container = document.createElement( 'div' );
//   document.body.appendChild( container );
    this.$refs.can.appendChild(container);
  camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
  camera.position.z = 1000;
  scene = new THREE.Scene();
  var numParticles = AMOUNTX * AMOUNTY;
  var positions = new Float32Array( numParticles * 3 );
  var scales = new Float32Array( numParticles );

  var i = 0, j = 0;
  for ( var ix = 0; ix < AMOUNTX; ix ++ ) {
    for ( var iy = 0; iy < AMOUNTY; iy ++ ) {
      positions[ i ] = ix * SEPARATION - ( ( AMOUNTX * SEPARATION ) / 2 ); // x
      positions[ i + 1 ] = 0; // y
      positions[ i + 2 ] = iy * SEPARATION - ( ( AMOUNTY * SEPARATION ) / 2 ); // z
      scales[ j ] = 1;
      i += 3;
      j ++;
    }
  }

  var geometry = new THREE.BufferGeometry();
  geometry.setAttribute( 'position', new THREE.BufferAttribute( positions, 3 ) );
  geometry.setAttribute( 'scale', new THREE.BufferAttribute( scales, 1 ) );

  var material = new THREE.ShaderMaterial( {
    uniforms: {
      color: { value: new THREE.Color( 0x0078de ) },
    },
    vertexShader: document.getElementById( 'vertexshader' ).textContent,
    fragmentShader: document.getElementById( 'fragmentshader' ).textContent

  } );
  particles = new THREE.Points( geometry, material );
  scene.add( particles );
  renderer = new THREE.WebGLRenderer( { antialias: true } );
  renderer.setPixelRatio( window.devicePixelRatio );
  renderer.setSize( window.innerWidth, window.innerHeight );
  renderer.setClearColor(0x141a48); // 设置背景色
  container.appendChild( renderer.domElement );

  // stats = new Stats();
  // container.appendChild( stats.dom );

  document.addEventListener( 'mousemove', onDocumentMouseMove, false );
  document.addEventListener( 'touchstart', onDocumentTouchStart, false );
  document.addEventListener( 'touchmove', onDocumentTouchMove, false );
  window.addEventListener( 'resize', onWindowResize, false );











  }
}

//**************** */
var SEPARATION = 100, AMOUNTX = 50, AMOUNTY = 50;

var container, stats;
var camera, scene, renderer;

var particles, count = 0;

var mouseX = 0, mouseY = 0;

var windowHalfX = window.innerWidth / 2;
var windowHalfY = window.innerHeight / 2;

init();
animate();

function init() {

  container = document.createElement( 'div' );
  document.body.appendChild( container );
    // this.$refs.can.appendChild(container);
  camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
  camera.position.z = 1000;
  scene = new THREE.Scene();
  var numParticles = AMOUNTX * AMOUNTY;
  var positions = new Float32Array( numParticles * 3 );
  var scales = new Float32Array( numParticles );

  var i = 0, j = 0;
  for ( var ix = 0; ix < AMOUNTX; ix ++ ) {
    for ( var iy = 0; iy < AMOUNTY; iy ++ ) {
      positions[ i ] = ix * SEPARATION - ( ( AMOUNTX * SEPARATION ) / 2 ); // x
      positions[ i + 1 ] = 0; // y
      positions[ i + 2 ] = iy * SEPARATION - ( ( AMOUNTY * SEPARATION ) / 2 ); // z
      scales[ j ] = 1;
      i += 3;
      j ++;
    }
  }

  var geometry = new THREE.BufferGeometry();
  geometry.setAttribute( 'position', new THREE.BufferAttribute( positions, 3 ) );
  geometry.setAttribute( 'scale', new THREE.BufferAttribute( scales, 1 ) );

  var material = new THREE.ShaderMaterial( {
    uniforms: {
      color: { value: new THREE.Color( 0x0078de ) },
    },
    vertexShader: document.getElementById( 'vertexshader' ).textContent,
    fragmentShader: document.getElementById( 'fragmentshader' ).textContent

  } );
  particles = new THREE.Points( geometry, material );
  scene.add( particles );
  renderer = new THREE.WebGLRenderer( { antialias: true } );
  renderer.setPixelRatio( window.devicePixelRatio );
  renderer.setSize( window.innerWidth, window.innerHeight );
  renderer.setClearColor(0x141a48); // 设置背景色
  container.appendChild( renderer.domElement );

  // stats = new Stats();
  // container.appendChild( stats.dom );

  document.addEventListener( 'mousemove', onDocumentMouseMove, false );
  document.addEventListener( 'touchstart', onDocumentTouchStart, false );
  document.addEventListener( 'touchmove', onDocumentTouchMove, false );
  window.addEventListener( 'resize', onWindowResize, false );

}

function onWindowResize() {

  windowHalfX = window.innerWidth / 2;
  windowHalfY = window.innerHeight / 2;

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize( window.innerWidth, window.innerHeight );

}

//

function onDocumentMouseMove( event ) {

  mouseX = event.clientX - windowHalfX;
  mouseY = event.clientY - windowHalfY;

}

function onDocumentTouchStart( event ) {

  if ( event.touches.length === 1 ) {

    event.preventDefault();

    mouseX = event.touches[ 0 ].pageX - windowHalfX;
    mouseY = event.touches[ 0 ].pageY - windowHalfY;

  }

}

function onDocumentTouchMove( event ) {

  if ( event.touches.length === 1 ) {

    event.preventDefault();

    mouseX = event.touches[ 0 ].pageX - windowHalfX;
    mouseY = event.touches[ 0 ].pageY - windowHalfY;

  }

}

//

function animate() {

  requestAnimationFrame( animate );

  render();
  // stats.update();

}

function render() {

  camera.position.x += ( mouseX - camera.position.x ) * .05;
  camera.position.y += ( - mouseY - camera.position.y ) * .05;
  camera.lookAt( scene.position );

  var positions = particles.geometry.attributes.position.array;
  var scales = particles.geometry.attributes.scale.array;

  var i = 0, j = 0;

  for ( var ix = 0; ix < AMOUNTX; ix ++ ) {

    for ( var iy = 0; iy < AMOUNTY; iy ++ ) {

      positions[ i + 1 ] = ( Math.sin( ( ix + count ) * 0.3 ) * 50 ) +
              ( Math.sin( ( iy + count ) * 0.5 ) * 50 );

      scales[ j ] = ( Math.sin( ( ix + count ) * 0.3 ) + 1 ) * 8 +
              ( Math.sin( ( iy + count ) * 0.5 ) + 1 ) * 8;

      i += 3;
      j ++;

    }

  }

  particles.geometry.attributes.position.needsUpdate = true;
  particles.geometry.attributes.scale.needsUpdate = true;

  renderer.render( scene, camera );

  count += 0.1;

}
//*************** */
</script>
<style lang="stylus" scoped></style>
<style>
.firstdemo {
  padding-top: 160px;
}
.login-container a {
  color: #0078de;
}
#canvascontainer {
  position: absolute;
  top: 0px;
}
</style>