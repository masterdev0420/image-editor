<template>
  <div>
    <div v-if="mSelectMode === 'one'" style="font-size: 12px" id="attribute">
      <!-- general properties -->
      <div
        class="right_header"
        style="
          position: sticky;
          width: 100%;
          top: 0px;
          z-index: 4;
          background-color: white;
        "
      >
        <!-- Right Head -->
        <right-header
          :mSelectOneTypeProps="[mSelectOneType, this.baseAttr]"
        ></right-header>
        <!-- Right Head -->

        <!-- layer setting -->
        <div class="mt-2 customborder"></div>
        <div class="mt-2" style="height: 40px; margin-right: 40px">
          <div style="float: left; margin-left: 30px"><layer1></layer1></div>
          <div style="float: right">
            <Tooltip
              v-if="mSelectMode === 'one'"
              content="Click to unlock or lock"
              placement="top"
            >
              <Button
                v-if="isLock"
                @click="doLock(false)"
                icon="md-lock"
                type="text"
              ></Button>
              <Button
                v-else
                @click="doLock(true)"
                icon="ios-unlock-outline"
                type="text"
              ></Button>
            </Tooltip>
            <Tooltip
              v-if="mSelectMode === 'one'"
              content="Click to view or unview"
              placement="top"
            >
              <Button
                v-if="isView"
                icon="ios-eye-off-outline"
                @click="doView(true)"
                type="text"
              ></Button>
              <Button
                v-else
                @click="doView(false)"
                icon="ios-eye-outline"
                type="text"
              ></Button>
            </Tooltip>
            <Dropdown placement="bottom-end">
              <Button icon="ios-more" type="text"> </Button>
              <template #list>
                <DropdownMenu>
                  <clone></clone>
                  <dele></dele>
                </DropdownMenu>
              </template>
            </Dropdown>
          </div>
        </div>
        <!-- layer setting -->
        <div class="customborder"></div>
      </div>
      <div class="setBox">
        <div v-show="imgType.includes(mSelectOneType)">
          <!-- control part -->
          <div class="control" style="margin-left: 15px">
            <div class="row mt-3 mb-3" style="align-items: center">
              <div class="col col-lg-7">Size</div>
              <div class="input-size">
                <div class="content">
                  <div
                    class="ivu-input-wrapper ivu-input-wrapper-default ivu-input-type-text"
                  >
                    <span class="ivu-input-suffix">
                      <i style="font-style: normal; font-size: 11px">w</i>
                      <!-- W -->
                    </span>
                    <input
                      style="width: 100%"
                      autocomplete="off"
                      type="number"
                      class="ivu-input ivu-input-with-suffix"
                      placeholder="Enter text"
                      v-model="baseAttr.width"
                      @change="(value) => changeCommon('width', value)"
                      number="true"
                    />
                  </div>
                </div>
              </div>
              <div class="input-size">
                <div class="content">
                  <div
                    class="ivu-input-wrapper ivu-input-wrapper-default ivu-input-type-text"
                  >
                    <span class="ivu-input-suffix">
                      <i style="font-style: normal; font-size: 11px">h</i>
                      <!-- H -->
                    </span>
                    <input
                      autocomplete="off"
                      type="number"
                      class="ivu-input ivu-input-default ivu-input-with-suffix"
                      placeholder="Enter text"
                      v-model="baseAttr.height"
                      @change="(value) => changeCommon('height', value)"
                      number="true"
                    />
                  </div>
                </div>
              </div>
            </div>

            <!-- --------------------- Position ----------------------->
            <div class="row mb-3" style="align-items: center">
              <div class="col col-lg-7">Position</div>
              <div class="input-size">
                <div class="content">
                  <div
                    class="ivu-input-wrapper ivu-input-wrapper-default ivu-input-type-text"
                  >
                    <span class="ivu-input-suffix">
                      <i style="font-style: normal; font-size: 11px">x</i>
                      <!-- X -->
                    </span>
                    <input
                      autocomplete="off"
                      type="number"
                      class="ivu-input ivu-input-default ivu-input-with-suffix"
                      placeholder="Enter text"
                      v-model="baseAttr.left"
                      @change="(value) => changeCommon('left', value)"
                      pattern="^\d*(\.\d{0,2})?$"
                    />
                  </div>
                </div>
              </div>
              <div class="input-size">
                <div class="content">
                  <div
                    class="ivu-input-wrapper ivu-input-wrapper-default ivu-input-type-text"
                  >
                    <span class="ivu-input-suffix">
                      <i style="font-style: normal; font-size: 11px">y</i>
                      <!-- Y -->
                    </span>
                    <input
                      autocomplete="off"
                      type="number"
                      class="ivu-input ivu-input-default ivu-input-with-suffix"
                      placeholder="Enter text"
                      v-model="baseAttr.top"
                      @change="(value) => changeCommon('top', value)"
                      number="true"
                    />
                  </div>
                </div>
              </div>
            </div>
            <!-- --------------------- End Position ----------------------->
          </div>
          <!-- control part -->
        </div>
        <div
          v-show="imgType.includes(mSelectOneType)"
          style="margin-left: 15px"
        >
          <div class="row mb-3">
            <flip></flip>
            <div class="input-size">
              <div
                class="ivu-input-wrapper ivu-input-wrapper-default ivu-input-type-text"
              >
                <span class="ivu-input-suffix">
                  <i style="font-style: normal; font-size: 11px">&deg;</i>
                  <!-- Degree -->
                </span>
                <input
                  autocomplete="off"
                  type="number"
                  class="ivu-input ivu-input-default ivu-input-with-suffix"
                  placeholder="Enter text"
                  v-model="baseAttr.angle"
                  :max="360"
                  @change="(value) => changeCommon('angle', value)"
                />
              </div>
            </div>
          </div>
          <!-- emptypattern part -->
          <div>
            <div
              v-if="
                baseAttr.id === 'productImage' || baseAttr.id === 'nonBgImage' || baseAttr.customType === 'extra_product'
              "
              class=""
              style="height: 40px; margin-right: 15px"
            >
              <div style="float: left">Remove background</div>
              <div style="float: right">
                <Switch
                  size="small"
                  v-model="nonBgImageState"
                  @on-change="nonproductImageChange"
                />
                
              </div>            
            </div>
            <div
              v-if="
                baseAttr.id === 'productImage' || baseAttr.id === 'nonBgImage' || baseAttr.customType === 'extra_product'
              "
              class=""
              style="height: 40px; margin-right: 15px"
            >

            <div style="float: left">Trim background</div>
            <div style="float: right">
            <Switch
              size="small"
              v-model="trimImageState"
              @on-change="trimproductImageChange"
            />
            
          </div>           
            </div>            
          </div>

          <div class="mt-4" style="height: 30px; margin-right: 15px">
            <div class="" style="float: left">Align Horizontal/Vertical</div>
            <div class="" style="float: right" v-if="baseAttr.id === 'nonBgImage' && baseAttr.id !== 'productImage'">
              <align-image></align-image>
            </div>
            <div v-if="baseAttr.id === 'nonBgImage' && baseAttr.customType === 'extra_product'" >
              <div class="mt-3" style="height:40px;">
                <div class="" style="float:right;">
                  <align-product :position="[this.baseAttr.position]"></align-product>
                </div>
              </div>              
            </div>
          </div>
        </div>

        <!----------------- shape and text ----------------------->

        <shape-type
          :mSelectOneTypeProps="[mSelectOneType, this.baseAttr]"
        ></shape-type>
        <text-type
          :mSelectOneTypeProps="[mSelectOneType, this.baseAttr, this.fontAttr]"
          @textGroupSet="textGroupSet($event)"
        ></text-type>

        <!----------------- shape and text ----------------------->

        <!-- Layer restriction -->
        <div class="customborder mt-3"></div>
        <div class="mt-4" style="margin-left: 15px">
          <div class="" style="height: 40px; margin-right: 15px">
            <div style="float: left">Layer time restriction</div>
            <div style="float: right">
              <Switch
                size="small"
                v-model="restrictionState"
                true-color="#13ce66"
              />
            </div>
          </div>
          <div v-if="restrictionState" class="" style="align-items: center">
            <div class="" style="height: 40px; margin-right: 15px">
              <div style="float: left">Generate</div>
              <div class="input-size" style="float: right">
                <Select
                  v-model="this.baseAttr.layerShowPeriod.mode"
                  @on-change="(value) => changeLayerShowPeriod('mode', value)"
                >
                  <Option
                    v-for="item in modes"
                    :value="item.value"
                    :key="item.value"
                    >{{ item.label }}</Option
                  >
                </Select>
              </div>
            </div>
            <div class="" style="height: 40px; margin-right: 15px">
              <div style="float: left">From</div>
              <div style="float: right">
                <DatePicker
                  type="date"
                  style="width: 100%"
                  placeholder="Start date"
                  v-model="this.baseAttr.layerShowPeriod.startDate"
                  @on-change="
                    (value) => changeLayerShowPeriod('startDate', value)
                  "
                />
              </div>
            </div>
            <div class="" style="height: 40px; margin-right: 15px">
              <div style="float: left">Until</div>
              <div style="float: right">
                <DatePicker
                  type="date"
                  style="width: 100%"
                  placeholder="End date"
                  v-model="this.baseAttr.layerShowPeriod.endDate"
                  @on-change="
                    (value) => changeLayerShowPeriod('endDate', value)
                  "
                />
              </div>
            </div>
            <div class="mt-3">
              <input
                type="text"
                class="ivu-input"
                value="Generated all the time."
                id="genLayerPeriod"
                readonly
              />
            </div>
          </div>
        </div>

        <!-- Layer restriction -->
      </div>
    </div>
  </div>
</template>

<script>
import select from "@/mixins/select";
import layer1 from "./layer1.vue";
import Color from "./color.vue";
import dele from "./del.vue";
import clone from "./clone.vue";
import flip from "./flip.vue";
import alignImage from "./alignImage.vue";
import alignProduct from "./alignProduct.vue";
import { nonBgImage, trimImage,trimandremove, productImage } from "@/utils/imgConstant";
import rightHeader from "./rightHeader.vue";
import shapeType from "./shapeType.vue";
import textType from "./textType.vue";
import group from "./group.vue";
import { fabric } from "fabric";

const lockAttrs = [
  "lockMovementX",
  "lockMovementY",
  "lockRotation",
  "lockScalingX",
  "lockScalingY",
];

export default {
  name: "ToolBar",
  mixins: [select],
  components: {
    Color,
    layer1,
    dele,
    clone,
    flip,
    alignImage,
    rightHeader,
    alignProduct,
    shapeType,
    textType,
    group,
  },
  data() {
    return {
      //layer restriction
      modes: [
        {
          value: "only",
          label: "only",
        },
        {
          value: "except",
          label: "except",
        },
      ],
      restrictionState: true,
      //layer restriction
      isLock: false,
      isView: true,
      nonBgImageState: false,
      trimImageState:false,
      imageStatus: "",

      // common element
      baseType: [
        "text",
        "i-text",
        "textbox",
        "triangle",
        "image",
        "group",
        "line",
        "arrow",
        "rect",
        "circle",
      ],
      // <!------------------------image element --->
      imgType: ["image"],
      // general properties
      baseAttr: {
        name: "",
        width: 0,
        height: 0,
        round: 0,
        sizeX: 0,
        sizeY: 0,
        opacity: 0,
        angle: 0,
        fill: "#fff",
        left: 0,
        top: 0,
        strokeWidth: 0,
        stroke: "",
        shadow: {
          color: "#ffffff",
          blur: 0,
          offsetX: 0,
          offsetY: 0,
        },
      },
      // font properties
      fontAttr: {
        string: "",
        fontSize: 0,
        fontFamily: "",
        lineHeight: 0,
        charSpacing: 0,
        fontWeight: "",
        textBackgroundColor: "#fff",
        textAlign: "",
        fontStyle: "",
        underline: false,
        linethrough: false,
        overline: false,
      },

      // font alignment
      textAlignList: ["left", "center", "right"],
      // Align icon
      textAlignListSvg: [
        '<svg t="1650441458823" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3554" width="18" height="18"><path d="M198.4 198.4h341.333333c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533334 19.2v57.6c0 8.533333-2.133333 14.933333-8.533334 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-341.333333c-8.533333 0-14.933333-2.133333-19.2-8.533334-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533333z m0 170.666667h569.6c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333h-569.6c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533333z m0 170.666666h454.4c8.533333 0 14.933333 2.133333 19.2 8.533334 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333h-454.4c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533334z m0 170.666667h625.066667c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-625.066667c-8.533333 0-14.933333-2.133333-19.2-8.533334-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533333z" p-id="3555"></path></svg>',
        '<svg t="1650441512015" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3704" width="18" height="18"><path d="M313.6 198.4h398.933333c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533334 19.2v57.6c0 8.533333-2.133333 14.933333-8.533334 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-398.933333c-8.533333 0-14.933333-2.133333-19.2-8.533334-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 10.666667-8.533333 19.2-8.533333z m-115.2 170.666667h625.066667c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333h-625.066667c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533333z m115.2 170.666666h398.933333c8.533333 0 14.933333 2.133333 19.2 8.533334 6.4 6.4 8.533333 12.8 8.533334 19.2v57.6c0 8.533333-2.133333 14.933333-8.533334 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333h-398.933333c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 10.666667-8.533333 19.2-8.533334z m-115.2 170.666667h625.066667c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-625.066667c-8.533333 0-14.933333-2.133333-19.2-8.533334-6.4-6.4-8.533333-12.8-8.533333-19.2v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 4.266667-4.266667 12.8-8.533333 19.2-8.533333z" p-id="3705"></path></svg>',
        '<svg t="1650441519862" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3854" width="18" height="18"><path d="M454.4 283.733333v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 6.4-6.4 12.8-8.533333 19.2-8.533333h341.333334c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-341.333334c-8.533333 0-14.933333-2.133333-19.2-8.533334-4.266667-4.266667-8.533333-10.666667-8.533333-19.2z m-226.133333 170.666667v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 6.4-6.4 12.8-8.533333 19.2-8.533333h569.6c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333H256c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-4.266667-8.533333-10.666667-8.533333-19.2z m113.066666 170.666667v-57.6c0-8.533333 2.133333-14.933333 8.533334-19.2 6.4-6.4 12.8-8.533333 19.2-8.533334h454.4c8.533333 0 14.933333 2.133333 19.2 8.533334 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533333h-454.4c-8.533333 0-14.933333-2.133333-19.2-8.533333-6.4-4.266667-8.533333-10.666667-8.533334-19.2z m-170.666666 170.666666v-57.6c0-8.533333 2.133333-14.933333 8.533333-19.2 6.4-6.4 12.8-8.533333 19.2-8.533333h625.066667c8.533333 0 14.933333 2.133333 19.2 8.533333 6.4 6.4 8.533333 12.8 8.533333 19.2v57.6c0 8.533333-2.133333 14.933333-8.533333 19.2-6.4 6.4-12.8 8.533333-19.2 8.533334h-625.066667c-8.533333 0-14.933333-2.133333-19.2-8.533334-6.4-4.266667-8.533333-10.666667-8.533333-19.2z" p-id="3855"></path></svg>',
      ],
      imageShowMode: "",
      layerShowPeriod: {
        mode: "",
        startDate: "",
        endDate: "",
      },
    };
  },

  created() {
    this.canvas.c.on("object:scaling", (opt) => {
      var activeObject = this.canvas.c.getActiveObject();
    
      if(opt.transform.corner == "br"){
        var target = opt.target;
        var pointer = this.canvas.c.getPointer(opt.e);          
        if (target && target.id === "productImage" && pointer.x !== target.left && pointer.y !== target.top || target.customType === "extra_product") {
          var scaleXChange = (pointer.x - target.left) / target.width;
          var scaleYChange = (pointer.y - target.top) / target.height;
          target.lockScalingFlip = true;
          target.set({
            scaleX: scaleXChange,
            scaleY: scaleYChange,
          });
          target.setCoords(); // Update the object's coordinates
        }       
      }

      if (activeObject.customType == "productImage" || activeObject.customType == "extra_product" ) {
        this.controlProductImage();
        this.fixTextPosition();
      }

    }),
    this.event.on("selectUpdate", () => {
      if (this.canvas.c.getActiveObject().name == "picture") {
        this.canvas.c.getActiveObject().clipState = this.imageShowMode;
      }
    });

    this.event.on("selectOne", (items) => {
      this.isLock = !items[0].hasControls;
      this.mSelectActive = items[0];
      var activeObject = this.canvas.c.getActiveObjects()[0];

      if (activeObject) {
        // base
        this.baseAttr.id = activeObject.id;
        this.baseAttr.position = activeObject.position;
        this.baseAttr.customType = activeObject.customType;
        this.baseAttr.item_name = activeObject.item_name;
        this.baseAttr.round = activeObject.get("rx");
        this.baseAttr.height = activeObject.get("height");
        this.baseAttr.width = activeObject.get("width");
        this.baseAttr.opacity = activeObject.get("opacity") * 100;
        this.baseAttr.fill = activeObject.get("fill");
        if (this.baseAttr.fill == null) {
          this.baseAttr.fill = "";
        }
        this.baseAttr.left = activeObject.get("left");
        this.baseAttr.top = activeObject.get("top");
        this.baseAttr.stroke = activeObject.get("stroke");
        if (this.baseAttr.stroke == null) {
          this.baseAttr.stroke = "";
        }
        this.baseAttr.shadow = activeObject.get("shadow") || {};
        this.baseAttr.angle = activeObject.get("angle") || 0;
        this.baseAttr.padding = activeObject.get("padding") || 0;
        this.baseAttr.layerShowPeriod = activeObject.get("layerShowPeriod");
        if (items[0].type == "group") {
          const groupObj = items[0];
          const rectObj = items[0]._objects[0];
          const textObj = items[0]._objects[1];
          //<!----------------- group property --------------->
          this.fontAttr.position = groupObj.position;
          this.fontAttr.fontLists = window.globalFonts;
          this.fontAttr.selected_text_manage_type =
            groupObj.texthandle || "automatic";
          //<!----------------- end group property --------------->

          //<!----------------- rect property --------------->
          this.fontAttr.rectFill = rectObj.get("fill") || "";
          this.fontAttr.round = rectObj.get("ry") || 0;
          this.fontAttr.stroke = rectObj.get("stroke") || "";
          this.fontAttr.strokeWidth = rectObj.get("strokeWidth") || 0;
          this.fontAttr.strokeLabel = rectObj.get("strokeLabel") || "solid";
          //<!----------------- end rect property --------------->

          //<!----------------- end text property --------------->
          this.fontAttr.fill = textObj.get("fill") || "";
          this.fontAttr.text = textObj.get("text") || "";
          this.fontAttr.fontSize = textObj.get("fontSize");
          this.fontAttr.fontFamily = textObj.get("fontFamily");
          this.fontAttr.underline = textObj.get("underline");
          this.fontAttr.overline = textObj.get("overline");
          this.fontAttr.fontStyle = textObj.get("fontStyle");
          this.fontAttr.fontWeight = textObj.get("fontWeight");
          this.fontAttr.selected_fontfamily = textObj.fontFamily;

          if(activeObject.customType == "productImage" || activeObject.customType == "extra_product"){
            switch(activeObject.bgState){
              case "productImage" : 
                this.nonBgImageState = false;
                this.trimImageState = false;
                break;
              case "nonAndTrimImage" : 
                this.nonBgImageState = true;
                this.trimImageState = true;
                break;
              case "nonBgImage": 
                this.nonBgImageState = true;
                this.trimImageState = false;
                break;
              case "trimBgImage" : 
                this.nonBgImageState = false;
                this.trimImageState = true;                               
                break;
            }
          }

          // //layerRestricion
          this.setLayerShowPeriod();
          setTimeout(() => {
            this.generateLayerPeriod();
          }, 300);
          // //layerRestricion
        }
      }
    });
  },

  mounted() {
    // setInterval(() => {
    //   this.canvas.editor.checkLayerPeriod();
    // }, 1000);
  },

  computed: {},

  methods: {
    controlProductImage() {
      const activeObject = this.canvas.c.getActiveObject(),
        w = activeObject.width * activeObject.scaleX,
        h = activeObject.height * activeObject.scaleY;
      activeObject
        .set({
          height: h,
          width: w,
          scaleX: 1,
          scaleY: 1,
        }).setCoords();
      
      var rectW = activeObject.width;
      var imageW = activeObject._objects[1].width;
      var image_scale_x = rectW / imageW;
      var rectH = activeObject.height;
      var imageH = activeObject._objects[1].height;
      var image_scale_y = rectH / imageH;

      if (activeObject.width <= activeObject.height) {
        activeObject._objects[1]
          .set({
            left:
              0 -
              (activeObject._objects[1].width *
                activeObject._objects[1].scaleX) /
                2,
            top: 0 - (activeObject._objects[1].height * image_scale_x) / 2,
          });
        activeObject._objects[1].set("scaleX", image_scale_x).setCoords();
        activeObject._objects[1].set("scaleY", image_scale_x).setCoords();
      } else {
        activeObject._objects[1]
          .set({
            left:
              0 -
              (activeObject._objects[1].width *
                activeObject._objects[1].scaleX) /
                2,
            top: 0 - (activeObject._objects[1].height * image_scale_y) / 2,
          });
        activeObject._objects[1].set("scaleX", image_scale_y).setCoords();
        activeObject._objects[1].set("scaleY", image_scale_y).setCoords();
      }
      
      this.canvas.c.renderAll();
    },
    fixTextPosition(){
          const activeObject =this.canvas.c.getActiveObject();
         
          if(activeObject.customType == "productImage" || activeObject.customType == "extra_product"){

            const w = activeObject.width * activeObject.scaleX,
            h = activeObject.height * activeObject.scaleY

            activeObject.set({
              'height'     : h,
              'width'      : w,
              'scaleX'     : 1,
              'scaleY'     : 1,
            });
            
            activeObject._objects[0].set({
              'height'     : h,
              'width'      : w,
              'scaleX'     : 1,
              'scaleY'     : 1,
              "left":0 - (activeObject.width) / 2,
              "top":0 - (activeObject.height) / 2
            });
            var rectW = activeObject.width;
            var textW = activeObject._objects[1].width ;           
            var textS = rectW/textW;
            if(textW>rectW){
            //   activeObject.set({
            //       position:{
            //         "positionX":"left",
            //         "positionY":activeObject.position.positionY
            //       }
            //   });									
            //   activeObject._objects[1].set("scaleX",textS);
            //   activeObject._objects[1].set("scaleY",textS);			
            }else{

              if(activeObject.originPoistion == "right"){
                activeObject.set({
                    position:{
                      "positionX":"right",
                      "positionY":activeObject.position.positionY
                    }
                });                      
              }		              
              if(activeObject.originPoistion == "xCenter"){
                activeObject.set({
                    position:{
                      "positionX":"xCenter",
                      "positionY":activeObject.position.positionY
                    }
                });                
              }
	
              if(textS <= 1){
                activeObject._objects[1].set("scaleX",textS).setCoords();
                activeObject._objects[1].set("scaleY",textS).setCoords();	
              }			
            }
            var position = this.canvas.editor.getPosition(activeObject);
            if(activeObject.position.positionX == "right"){
              activeObject._objects[1].set({
                "left":position.left,
                "top":position.top
              });				
            }

            if(activeObject.position.positionX == "left"){
              activeObject._objects[1].set({
                "left":position.left,
                "top":position.top
              });				
            }	

            if(activeObject.position.positionX == "xCenter"){
              activeObject._objects[1].set({
                "left":position.left,
                "top":position.top
              });				
            }			
            if(activeObject.position.positionY == "top"){
              activeObject._objects[1].top = position.top
            }

            if(activeObject.position.positionY == "bottom"){
              activeObject._objects[1].top = position.top
            }	

            if(activeObject.position.positionY == "yCenter"){
              activeObject._objects[1].top = position.top
            }
           this.canvas.c.renderAll();
          }            
        },        
        
    setLayerShowPeriod() {
      var state = this.baseAttr.layerShowPeriod;

      if (state.mode == "" && state.startDate == "" && state.endDate == "") {
        this.restrictionState = false;
      } else {
        this.restrictionState = true;
      }
    },

    changeLayerShowPeriod(key, value) {
      var activeObject = this.canvas.c.getActiveObject();
      switch (key) {
        case "mode":
          activeObject.layerShowPeriod.mode = value;
          this.baseAttr.layerShowPeriod.mode = value;
          break;

        case "startDate":
          activeObject.layerShowPeriod.startDate = value;
          this.baseAttr.layerShowPeriod.startDate = value;
          break;

        case "endDate":
          activeObject.layerShowPeriod.endDate = value;
          this.baseAttr.layerShowPeriod.endDate = value;
          break;
      }
      this.generateLayerPeriod();
      this.canvas.c.renderAll();
    },

    generateLayerPeriod() {
      if (document.getElementById("genLayerPeriod") != null) {
        if (this.baseAttr.layerShowPeriod.startDate != "") {
          if (this.baseAttr.layerShowPeriod.mode == "except") {
            document.getElementById("genLayerPeriod").value =
              "Generation stopped from " +
              this.baseAttr.layerShowPeriod.startDate;
          } else {
            document.getElementById("genLayerPeriod").value =
              "Generated from " + this.baseAttr.layerShowPeriod.startDate;
          }
        }

        if (this.baseAttr.layerShowPeriod.endDate != "") {
          if (this.baseAttr.layerShowPeriod.mode == "except") {
            document.getElementById("genLayerPeriod").value =
              "Generation stopped until " +
              this.baseAttr.layerShowPeriod.endDate;
          } else {
            document.getElementById("genLayerPeriod").value =
              "Generated until " + this.baseAttr.layerShowPeriod.endDate;
          }
        }
        if (
          this.baseAttr.layerShowPeriod.startDate != "" &&
          this.baseAttr.layerShowPeriod.endDate != ""
        ) {
          if (this.baseAttr.layerShowPeriod.mode == "except") {
            document.getElementById("genLayerPeriod").value =
              "Generation stopped from " +
              this.baseAttr.layerShowPeriod.startDate +
              " until " +
              this.baseAttr.layerShowPeriod.endDate;
          } else {
            document.getElementById("genLayerPeriod").value =
              "Generated from " +
              this.baseAttr.layerShowPeriod.startDate +
              " until " +
              this.baseAttr.layerShowPeriod.endDate;
          }
        }

        if (
          this.baseAttr.layerShowPeriod.startDate == "" &&
          this.baseAttr.layerShowPeriod.endDate == ""
        ) {
          document.getElementById("genLayerPeriod").value =
            "Generated all the time";
        }
      }
    },

    nonproductImageChange(evt) {
      if (evt == true) {
        this.nonBgImageState = true;
        return true;
      } else {
        this.nonBgImageState = false;
        return true;
      }
    },
    trimproductImageChange(evt){

    },

    doView(isView) {
      isView ? this.view() : this.unView();
    },

    view() {
      this.isView = false;
      this.changeCommon("opacity", 0);
    },

    unView() {
      this.isView = true;
      this.changeCommon("opacity", 100);
    },

    doLock(isLock) {
      isLock ? this.lock() : this.unLock();
    },

    lock() {
      // Modify custom properties
      this.mSelectActive.hasControls = false;
      // Moify default properties
      lockAttrs.forEach((key) => {
        this.mSelectActive[key] = true;
      });

      this.mSelectActive.selectable = false;

      this.isLock = true;
      this.canvas.c.renderAll();
    },

    unLock() {
      // Modify custom properties
      this.mSelectActive.hasControls = true;
      // Modify default properties
      lockAttrs.forEach((key) => {
        this.mSelectActive[key] = false;
      });
      this.mSelectActive.selectable = true;
      this.isLock = false;
      this.canvas.c.renderAll();
    },

    imgWidthClip(width) {
      var imgW = this.canvas.c.getActiveObject()._objects[1].getElement().width;
      var imgH = this.canvas.c
        .getActiveObject()
        ._objects[1].getElement().height;
      const activeObject = this.canvas.c.getActiveObjects()[0];
      var scale = activeObject._objects[1].scaleX;
      activeObject.set("width", width);
      activeObject._objects[1].set("scaleX", scale);
      activeObject._objects[1].set("scaleY", scale);
      if (scale <= width / imgW) {
        scale = width / imgW;
        activeObject._objects[1].set("scaleX", scale);
        activeObject._objects[1].set("scaleY", scale);
        var top = 0 - (imgH * scale) / 2;
        var left = -(activeObject.width / 2);
        activeObject._objects[1].set("left", left);
        activeObject._objects[1].set("top", top);
      }
      var clipRect = new fabric.Rect({
        originX: "left",
        originY: "top",
        left: activeObject.left,
        top: activeObject.top,
        width: activeObject.width,
        height: activeObject.height,
        angle: activeObject.angle,
        absolutePositioned: true,
      });
      activeObject.clipPath = clipRect;

      this.canvas.c.renderAll();
      activeObject.clipPath = "";
      return;
    },

    imgHeightClip(height) {
      const activeObject = this.canvas.c.getActiveObjects()[0];
      var imgW = this.canvas.c.getActiveObject()._objects[1].getElement().width;
      var imgH = this.canvas.c
        .getActiveObject()
        ._objects[1].getElement().height;
      var scale = this.baseAttr.width / imgW;
      var top = 0 - (imgH * scale) / 2;
      var left = -(activeObject.width / 2);
      var diff = left + (activeObject.width / 2) * scale;

      if (scale <= height / imgH) {
        scale = height / imgH;
        top = 0 - (imgH * scale) / 2;
        left = -((activeObject._objects.width * activeObject.scaleX) / 2);
      }

      activeObject.set("height", height);
      activeObject._objects[1].set("scaleX", scale);
      activeObject._objects[1].set("scaleY", scale);
      activeObject._objects[1].set("left", left);
      activeObject._objects[1].set("top", top);

      var clipRect = new fabric.Rect({
        originX: "left",
        originY: "top",
        left: activeObject.left,
        top: activeObject.top,
        width: activeObject.width,
        height: activeObject.height,
        angle: activeObject.angle,
        absolutePositioned: true,
      });

      activeObject.clipPath = clipRect;
      this.canvas.c.renderAll();
      activeObject.clipPath = "";
      this.canvas.c.renderAll();
      return;
    },
    // Generic property changes
    changeCommon(key, evt) {
      const activeObject = this.canvas.c.getActiveObjects()[0];
      if (key === "fill") {
        activeObject.set(key, evt);
        this.canvas.c.renderAll();
        return;
      }
      if (key === "stroke") {
        activeObject.set(key, evt);
        this.canvas.c.renderAll();
        return;
      }

      if (key == "width") {
        if (this.imageShowMode == "clip") {
          this.imgWidthClip(Number(evt.target.value));
          return;
        }
      }
      if (key == "height") {
        if (this.imageShowMode == "clip") {
          this.imgHeightClip(Number(evt.target.value));
          return;
        }
      }
      // Transparency special conversion
      if (key === "opacity") {
        if (typeof evt == "number") {
          evt = Number(evt);
        } else {
          evt = Number(evt.target.value);
        }
        activeObject && activeObject.set(key, evt / 100);
        this.canvas.c.renderAll();
        return;
      }
      // Rotation Angle Adaptation
      if (key === "angle") {
        activeObject.rotate(Number(evt.target.value));
        this.canvas.c.renderAll();
        return;
      }
      if (key == "round") {
        activeObject.set("ry", Number(evt.target.value));
        activeObject.set("rx", Number(evt.target.value));
        this.canvas.c.renderAll();
        return;
      }
      activeObject && activeObject.set(key, Number(evt.target.value));
      this.controlProductImage();
      this.canvas.c.renderAll();
    },
    // border settings

    borderSet(key) {
      const activeObject = this.canvas.c.getActiveObjects()[0];
      if (activeObject) {
        const stroke = this.strokeDashList.find((item) => item.label === key);
        activeObject.set(stroke.value);
        this.canvas.c.renderAll();
      }
    },
    insertEmpty(file, bgState) {
      var id = this.canvas.c.getActiveObject().id;
      var customType = this.canvas.c.getActiveObject().customType;

      var originLeft = this.canvas.c.getActiveObject().left;
      var originTop = this.canvas.c.getActiveObject().top;
      var originHeight = this.canvas.c.getActiveObject().height;
      var originWidth = this.canvas.c.getActiveObject().width;
      var originScaleX = this.canvas.c.getActiveObject().scaleX;
      var originScaleY = this.canvas.c.getActiveObject().scaleY;

      var rectLeft = this.canvas.c.getActiveObject()._objects[0].left;
      var rectTop = this.canvas.c.getActiveObject()._objects[0].top;
      var rectHeight = this.canvas.c.getActiveObject()._objects[0].height;
      var rectWidth = this.canvas.c.getActiveObject()._objects[0].width;
      var rectScaleX = this.canvas.c.getActiveObject()._objects[0].scaleX;
      var rectScaleY = this.canvas.c.getActiveObject()._objects[0].scaleY;

      var imageLeft = this.canvas.c.getActiveObject()._objects[1].left;
      var imageTop = this.canvas.c.getActiveObject()._objects[1].top;
      var imageScaleX = this.canvas.c.getActiveObject()._objects[1].scaleX;
      var imageScaleY = this.canvas.c.getActiveObject()._objects[1].scaleY;

      var item_name = this.canvas.c.getActiveObject().item_name;
      var angle = this.canvas.c.getActiveObject().angle;
      var opacity = this.canvas.c.getActiveObject().opacity;
      var layerShowPeriod = this.canvas.c.getActiveObject().layerShowPeriod;            
      var position = this.canvas.c.getActiveObject().position;
      var originPoistion = this.canvas.c.getActiveObject().originPoistion;
      const imgEl = document.createElement("img");
      imgEl.src = file;
      document.body.appendChild(imgEl);
      imgEl.onload = () => {
        // Create a product image
        if(customType == "extra_product"){
          var tempImageLeft = this.canvas.c.getActiveObject()._objects[1]._objects[0].left;
          var tempImageTop = this.canvas.c.getActiveObject()._objects[1]._objects[0].top;
          var tempImageScaleX = this.canvas.c.getActiveObject()._objects[1]._objects[0].scaleX;
          var tempImageScaleY = this.canvas.c.getActiveObject()._objects[1]._objects[0].scaleY;

          var textLeft = this.canvas.c.getActiveObject()._objects[1]._objects[1].left;
          var textTop = this.canvas.c.getActiveObject()._objects[1]._objects[1].top;
          var textScaleX = this.canvas.c.getActiveObject()._objects[1]._objects[1].scaleX;
          var textScaleY = this.canvas.c.getActiveObject()._objects[1]._objects[1].scaleY;        

          var tempImage = new this.fabric.Image(imgEl, {
            id: id,
            item_name: item_name,
            left: tempImageLeft,
            top: tempImageTop,
            scaleX: tempImageScaleX,
            scaleY: tempImageScaleY
          });

          var text = new this.fabric.IText(item_name, {
            fontFamily: 'Courier New',
            fontSize: 20,
            fontWeight:"bold",
            fill:"white",
            left:textLeft,
            top:textTop,
            scaleX:textScaleX,
            scralY:textScaleY,                
          });          

          var imgInstance = new this.fabric.Group([tempImage, text]);
          imgInstance.set({
            id: id,
            customType:customType,
            item_name: item_name,
            left: imageLeft,
            top: imageTop,
            scaleX: imageScaleX,
            scaleY: imageScaleY,
            opacity: opacity,
          });          
        }

        if(customType == "productImage"){
    
          var imgInstance = new this.fabric.Image(imgEl, {
            id: id,
            item_name: item_name,
            left: imageLeft,
            top: imageTop,
            scaleX: imageScaleX,
            scaleY: imageScaleY,
            opacity: opacity,
          });
        }

        var rect = new fabric.Rect({
          left: rectLeft,
          top: rectTop,
          width: rectWidth,
          height: rectHeight,
          scaleX: rectScaleX,
          scaleY: rectScaleY,
          fill: "",
          strokeWidth: 0,
          opacity: 100,
        });

        var group = new fabric.Group([rect, imgInstance], {
          id: id,
          bgState: bgState,
          item_name: item_name,
          left: originLeft,
          top: originTop,
          width: originWidth,
          height: originHeight,
          scaleX: originScaleX,
          scaleY: originScaleY,
          layerShowPeriod: layerShowPeriod,
          angle: angle,
          customType: customType,
          objectCaching: false,
          position:position,
          originPoistion:originPoistion,
        });
        group.setControlsVisibility({
          tl: false, // Hide top left point
          // tr: false, // Hide top right point
          bl: false, // Hide bottom left point
          mt: false, // Hide middle top point
          ml: false, // Hide middle left point
          mr: false, // Hide middle right point
          mb: false, // Hide middle bottom point
          mtr: false, // Hide rotation control button
        });       
        var objects = this.canvas.c.getObjects();
        if(customType == "prodcutImage"){
          var productIndex = objects.findIndex((el) => {
            return el.customType == "productImage";
          });
        }else{
          var productIndex = objects.findIndex((el) => {
            return el.customType == "extra_product";
          });          
        }
        
        productIndex = productIndex + 1;
        this.canvas.c.remove(this.canvas.c.getActiveObjects()[0]);
        this.canvas.c.add(group);
        this.canvas.c.setActiveObject(group);

        for (var i = 0; i < objects.length - productIndex; i++) {
          const activeObject = this.canvas.c.getActiveObject();
          activeObject && activeObject.sendBackwards();
        }

        imgEl.remove();
        this.canvas.c.renderAll();
        // this.controlProductImage();
        
      };
    },
  },
  watch: {
    nonBgImageState() {
      if(this.trimImageState == true && this.nonBgImageState == true){
        this.insertEmpty(trimandremove, "nonAndTrimImage");
      }
      if(this.trimImageState == false && this.nonBgImageState == true){
        this.insertEmpty(nonBgImage, "nonBgImage");
      } 
      if(this.trimImageState == true && this.nonBgImageState == false){
        this.insertEmpty(trimImage, "trimBgImage");
      }    
      if(this.trimImageState == false && this.nonBgImageState == false){
        this.insertEmpty(productImage, "productImage");
      } 
    },
    trimImageState(){
      if(this.trimImageState == true && this.nonBgImageState == true){
        this.insertEmpty(trimandremove, "nonAndTrimImage");
      }
      if(this.trimImageState == false && this.nonBgImageState == true){
        this.insertEmpty(nonBgImage, "nonBgImage");
      } 
      if(this.trimImageState == true && this.nonBgImageState == false){
        this.insertEmpty(trimImage, "trimBgImage");
      }    
      if(this.trimImageState == false && this.nonBgImageState == false){
        this.insertEmpty(productImage, "productImage");
      }         
    }
  },
};
</script>

<style scoped lang="less">
.input-size {
  width: 20%;
}
Button {
  padding: 0px 5px;
}
.box {
  width: 100%;
}

.flex-item {
  display: inline-flex;
  flex: 1;
  .label {
    width: 32px;
    height: 32px;
    line-height: 32px;
    display: inline-block;
    font-size: 14px;
    color: #333333;
  }
}
</style>
