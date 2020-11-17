
<template>
  <div id="app">
      <DeckGl 
            ref="deck"
            :class="['fill-wrapper']"
            :layers="layers"
            :canvas="'deck-canvas'"
            :width="'100%'"
            :height="'100%'"
            :controller="true"
            :useDevicePixels="false"
            :viewState="viewState"
            :views="deckglSettings.views"
            :parameters="{clearColor: [124, 200, 230, 1]}"
            @initialRender="()=>{hasDeckLoaded = true}"
            />
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */
import DeckGl from '@hirelofty/vue_deckgl'
import {PointCloudLayer} from '@deck.gl/layers';
import {COORDINATE_SYSTEM} from '@deck.gl/core';
import {PLYWorkerLoader} from '@loaders.gl/ply';
import {OBJWorkerLoader} from '@loaders.gl/obj'
import {registerLoaders} from '@loaders.gl/core';
import {OrbitView} from '@deck.gl/core';

const INITIAL_VIEW_STATE = {
    target: [0, 0, 0],
    rotationX: 0,
    rotationOrbit: 0,
    orbitAxis: 'Y',
    fov: 50,
    minZoom: 0,
    maxZoom: 10,
    zoom: .1
  };

const DECKGL_SETTINGS = {
    canvas: "deck-canvas",
    width: "100%",
    height: "100%",
    controller: true,
    useDevicePixels: false,
    viewState: INITIAL_VIEW_STATE,
    views: new OrbitView(),
}



const SAMPLE_FILES = {
      ply_sample:'https://raw.githubusercontent.com/visgl/deck.gl-data/master/examples/point-cloud-ply/lucy800k.ply',
      obj_sample:'https://raw.githubusercontent.com/wsmoyer/deckgl-sample-files/main/Lobster.obj'
}
  



export default {
  name: 'App',
  data(){
    return {
        deckglSettings: DECKGL_SETTINGS,
        layers:[],
        hasDeckLoaded: false,
        viewState: DECKGL_SETTINGS.viewState
    }
  },
  mounted(){
      registerLoaders(OBJWorkerLoader);
   
      const SAMPLE_DATA = SAMPLE_FILES.obj_sample
      this.layers.push (new PointCloudLayer({
      id: 'laz-point-cloud-layer',
      data: SAMPLE_DATA,
      onDataLoad: ({header}) => this.onDataLoad({header}),
      coordinateOrigin: [0, 25],
      coordinateSystem: COORDINATE_SYSTEM.CARTESIAN,
      getNormal: [0, 1, 0],
      getColor: [138, 227, 212],
      opacity: 0.9,
      pointSize: 0.7
    }))
  },
  components: {
    DeckGl,
  },
     
}
</script>