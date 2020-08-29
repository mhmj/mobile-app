<template>
    <nb-container>
        <nb-content>
            <scroll-view class="container" :content-container-style="{ flexGrow: 1,justifyContent: 'center'}">
                <barcode-component v-if="scannerActive" v-on:qrScanned="scannedData($event)"></barcode-component>
                <view v-if="!scannerActive & !cameraSettings.show">
                    <view class="btn-container">
                        <touchable-opacity  class="scan-btn" style="background-color: #f9dc6b;" :on-press="() => toggleScanner(1)">
                            <text class="btn-text">Receive</text>
                        </touchable-opacity>
                    </view>
                    <view class="icon-container" :style="{marginTop: 50}">
                        <image
                                :style="{width: 150, height: 150}"
                                :source="{uri: 'https://cdn1.iconfinder.com/data/icons/banking-36/128/qr__code_coding_scan_qrcode-512.png'}"
                        />
                    </view>
                    <view class="btn-container">
                        <touchable-opacity  class="scan-btn" style="background-color: #4d6286;" :on-press="() => toggleScanner(2)">
                            <text class="btn-text">Measure</text>
                        </touchable-opacity>
                        <touchable-opacity  class="scan-btn" style="background-color: #69b6f3;">
                            <text class="btn-text">Pack</text>
                        </touchable-opacity>
                    </view>
                    <view class="btn-container">
                        <touchable-opacity  class="scan-btn" style="background-color: #ea336b;">
                            <text class="btn-text">Shipping</text>
                        </touchable-opacity>
                        <touchable-opacity  class="scan-btn" style="background-color: #47b5ab;">
                            <text class="btn-text">Deliver</text>
                        </touchable-opacity>
                    </view>
                </view>
                <!--<view v-if="!cameraSettings.show && !preview">-->
                    <!--<view v-if="!scannerActive && action === null">-->
                        <!--<view class="btn-container">-->
                            <!--<touchable-opacity  class="scan-btn" style="background-color: #f9dc6b;" :on-press="() => toggleScanner(1)">-->
                                <!--<text class="btn-text">Receive</text>-->
                            <!--</touchable-opacity>-->
                        <!--</view>-->
                        <!--<view class="icon-container" :style="{marginTop: 50}">-->
                            <!--<image-->
                                    <!--v-if="!scannerActive"-->
                                    <!--:style="{width: 150, height: 150}"-->
                                    <!--:source="{uri: 'https://cdn1.iconfinder.com/data/icons/banking-36/128/qr__code_coding_scan_qrcode-512.png'}"-->
                            <!--/>-->
                        <!--</view>-->
                        <!--<view class="btn-container">-->
                            <!--<touchable-opacity  class="scan-btn" style="background-color: #4d6286;" :on-press="() => toggleScanner(2)">-->
                                <!--<text class="btn-text">Measure</text>-->
                            <!--</touchable-opacity>-->
                            <!--<touchable-opacity  class="scan-btn" style="background-color: #69b6f3;">-->
                                <!--<text class="btn-text">Pack</text>-->
                            <!--</touchable-opacity>-->
                        <!--</view>-->
                        <!--<view class="btn-container">-->
                            <!--<touchable-opacity  class="scan-btn" style="background-color: #ea336b;">-->
                                <!--<text class="btn-text">Shipping</text>-->
                            <!--</touchable-opacity>-->
                            <!--<touchable-opacity  class="scan-btn" style="background-color: #47b5ab;">-->
                                <!--<text class="btn-text">Deliver</text>-->
                            <!--</touchable-opacity>-->
                        <!--</view>-->
                    <!--</view>-->
                    <!--<view v-if="!scannerActive && action === 1">-->
                        <!--<view class="btn-container" >-->
                            <!--<touchable-opacity :class="[{'b-grey': deliveryType === 1}, {'b-info': deliveryType === 0}, 'b-a' ]"  :on-press="() => deliveryType = 0">-->
                                <!--<image-->
                                        <!--:style="{width: 70, height: 70}"-->
                                        <!--:source="{uri: 'https://www.pngrepo.com/png/269085/170/courier.png'}"-->
                                <!--/>-->
                                <!--<text style="text-transform: uppercase;">Hired Driver</text>-->
                            <!--</touchable-opacity>-->
                            <!--<touchable-opacity  :class="[{'b-grey': deliveryType === 0}, {'b-info': deliveryType === 1}, 'b-a' ]" :on-press="() => deliveryType = 1">-->
                                <!--<image-->
                                        <!--:style="{width: 70, height: 70}"-->
                                        <!--:source="{uri: 'https://icon-library.net/images/courier-icon/courier-icon-14.jpg'}"-->
                                <!--/>-->
                                <!--<text style="text-transform: uppercase;">Courier Service</text>-->
                            <!--</touchable-opacity>-->
                        <!--</view>-->
                        <!--<view v-if="deliveryType === 0">-->
                            <!--<nb-form>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Quantity</nb-label>-->
                                    <!--<nb-input />-->
                                <!--</nb-item>-->
                            <!--</nb-form>-->
                            <!--<nb-content padder>-->
                                <!--<nb-button block light style="backgroundColor: #47b5ab; text-transform: uppercase;" :on-press="receiveOrder">-->
                                    <!--<nb-text>Confirm</nb-text>-->
                                <!--</nb-button>-->
                                <!--<nb-button block light style="marginTop: 10; backgroundColor: #c8c8c8; text-transform: uppercase;" :on-press="() => action = null">-->
                                    <!--<nb-text>Cancel</nb-text>-->
                                <!--</nb-button>-->
                            <!--</nb-content>-->
                        <!--</view>-->
                        <!--<view v-if="deliveryType === 1">-->
                            <!--<view>-->
                                <!--<nb-form>-->
                                    <!--<nb-item stackedLabel>-->
                                        <!--<nb-label style="text-transform: uppercase;">Quantity</nb-label>-->
                                        <!--<nb-input />-->
                                    <!--</nb-item>-->
                                    <!--<nb-item stackedLabel>-->
                                        <!--<nb-label style="text-transform: uppercase;">Courier Service Name</nb-label>-->
                                        <!--<nb-input v-model="warehouseReceive.courier" />-->
                                    <!--</nb-item>-->
                                    <!--<nb-item stackedLabel>-->
                                        <!--<nb-label style="text-transform: uppercase;">Consignment Tracking No</nb-label>-->
                                        <!--<nb-input v-model="warehouseReceive.tracking_number" />-->
                                    <!--</nb-item>-->
                                    <!--<nb-item stackedLabel>-->
                                        <!--<nb-label style="text-transform: uppercase;">Consignment Attachment</nb-label>-->
                                        <!--<view :style="{left: '-45%',paddingTop: 20, paddingLeft:20}">-->
                                            <!--<view class="box" v-if="!!previewImage.uri===false">-->
                                                <!--<touchable-opacity :on-press="openCamera">-->
                                                    <!--<nb-thumbnail square-->
                                                          <!--:style="{marginRight:15}"-->
                                                          <!--:source="{uri: 'https://www.searchpng.com/wp-content/uploads/2019/02/Camera-Icon-PNG-715x544.png'}"-->
                                                    <!--/>-->
                                                <!--</touchable-opacity>-->
                                            <!--</view>-->
                                            <!--<view class="thumbnail" v-if="!!previewImage.uri===true">-->
                                                <!--<touchable-opacity>-->
                                                    <!--<image-->
                                                            <!--:style="{width: 50, height: 50}"-->
                                                            <!--:source="previewImage"-->
                                                    <!--/>-->
                                                <!--</touchable-opacity>-->
                                            <!--</view>-->
                                        <!--</view>-->
                                    <!--</nb-item>-->
                                    <!--<nb-content padder>-->
                                        <!--<nb-button block light style="backgroundColor: #47b5ab; text-transform: uppercase;" :on-press="receiveOrder">-->
                                            <!--<nb-text>Confirm</nb-text>-->
                                        <!--</nb-button>-->
                                        <!--<nb-button block light style="marginTop: 10; backgroundColor: #c8c8c8; text-transform: uppercase;" :on-press="() => action = null">-->
                                            <!--<nb-text>Cancel</nb-text>-->
                                        <!--</nb-button>-->
                                    <!--</nb-content>-->
                                <!--</nb-form>-->
                            <!--</view>-->
                        <!--</view>-->
                    <!--</view>-->
                    <!--<view v-if="!scannerActive && action === 2">-->
                        <!--<view class="btn-container">-->
                            <!--<touchable-opacity :class="[{'b-grey': packages.type === 1}, {'b-info': packages.type === 0}, 'b-a' ]"   :on-press="() => packages.type = 0">-->
                                <!--<image-->
                                        <!--:style="{width: 70, height: 70}"-->
                                        <!--:source="{uri: 'https://www.pngarts.com/files/1/Package-Box-PNG-Free-Download.png'}"-->
                                <!--/>-->
                                <!--<text class="btn-text">Carton</text>-->
                            <!--</touchable-opacity>-->
                            <!--<touchable-opacity :class="[{'b-grey': packages.type === 0}, {'b-info': packages.type === 1}, 'b-a' ]"  :on-press="() => packages.type = 1">-->
                                <!--<image-->
                                        <!--:style="{width: 70, height: 70}"-->
                                        <!--:source="{uri: 'http://iconbug.com/data/6e/256/2f229a2663bc61dfe5aedeace2ca8837.png'}"-->
                                <!--/>-->
                                <!--<text class="btn-text">Pallete</text>-->
                            <!--</touchable-opacity>-->
                        <!--</view>-->
                        <!--<view v-if="packages.type !== ''">-->
                            <!--<nb-form>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Length</nb-label>-->
                                    <!--<nb-input v-model="packages.length" />-->
                                <!--</nb-item>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Width</nb-label>-->
                                    <!--<nb-input v-model="packages.width" />-->
                                <!--</nb-item>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Height</nb-label>-->
                                    <!--<nb-input v-model="packages.height" />-->
                                <!--</nb-item>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Quantity</nb-label>-->
                                    <!--<nb-input v-model="packages.quantity" />-->
                                <!--</nb-item>-->
                                <!--<nb-item stackedLabel>-->
                                    <!--<nb-label style="text-transform: uppercase;">Description</nb-label>-->
                                    <!--<nb-input v-model="packages.description" />-->
                                <!--</nb-item>-->
                            <!--</nb-form>-->
                            <!--<nb-content padder>-->
                                <!--<nb-button block light style="backgroundColor: #47b5ab; text-transform: uppercase;" :on-press="onSubmitMeasure">-->
                                    <!--<nb-text>Submit</nb-text>-->
                                <!--</nb-button>-->
                                <!--<nb-button block light style="marginTop: 10; backgroundColor: #c8c8c8; text-transform: uppercase;" :on-press="() => action = null">-->
                                    <!--<nb-text>Cancel</nb-text>-->
                                <!--</nb-button>-->
                            <!--</nb-content>-->
                        <!--</view>-->
                    <!--</view>-->
                <!--</view>-->
                <!--<view v-else="!cameraSettings.show && preview" class="container" :style="{ backgroundColor: 'black', justifyContent: 'center'}">-->
                    <!--<view style="top: 0; paddingTop: 30; justifyContent: flex-end; alignItems: flex-start; right: -90%;">-->
                        <!--<image-->
                                <!--:style="{width: 40, height: 40}"-->
                                <!--:source="{uri: 'https://cdn4.iconfinder.com/data/icons/defaulticon/icons/png/256x256/stop.png'}"-->
                        <!--/>-->
                    <!--</view>-->
                    <!--<image-->
                            <!--:style="{width: 500, height: 600, alignItems: 'center'}"-->
                            <!--:source="previewImage"-->
                    <!--/>-->
                <!--</view>-->
            </scroll-view>
        </nb-content>
    </nb-container>
    <cameraComponent></cameraComponent>
</template>
<script>

    import Vue from "vue-native-core";
    import { VueNativeBase } from "native-base";
    import { Toast } from "native-base";

    import cameraComponent from './components/cameraComponent.vue';
    import barcodeComponent from './components/barcodeComponent.vue'

    // registering all native-base components to the global scope of the Vue
    Vue.use(VueNativeBase);
    import { Camera,Permissions,BarCodeScanner } from "expo";


    export default  {
        data() {
            return{
                //serverurl: 'www.izyim.com',
                //serverurl: '3f7fba39.ngrok.io',
                serverurl: 'c116033c.ngrok.io',
                hasCameraPermission: null,
                type: BarCodeScanner.Constants.Type.back,
                scannerActive: false,
                orderId : null,
                action: null,
                deliveryType: 0,
                warehouseReceive: {
                    courier: '',
                    tracking_number: ''
                },
                packages: {
                    description: '',
                    type:0,
                    quantity: 1,
                    width: 0.0,
                    height: 0.0,
                    length: 0.0
                },
                cameraSettings: {
                    show: false,
                    type: Camera.Constants.Type.back,
                    permissionsGranted: false,
                },
                previewImage: {
                    uri: null
                },
                preview : false,
            };
        },
        async componentDidMount() {
            this.getPermissionsAsync();
        },
        mounted: function() {
            Permissions.askAsync(Permissions.CAMERA)
                .then(status => {
                    hasCameraPermission = status.status == "granted" ? true : false;

                }).catch((err)=>{
                console.log(err);
            });
        },
        components: {
            Camera,
            BarCodeScanner,
            barcodeComponent,
            cameraComponent,
        },
        methods: {
            toggleScanner(action){
                alert('yes')
                this.scannerActive = !this.scannerActive;
                this.action = action;
            },
            IsValidJSONString(str) {
                try {
                    JSON.parse(str);
                } catch (e) {
                    return false;
                }
                return true;
            },
            receiveOrder(){
                if(this.deliveryType === 1){
                    this.updateTrackingNo()
                }
                this.updateOrder()
            },
            updateOrder(){
                fetch('https://'+ this.serverurl +'/api/v1/order/'+ this.orderId +'/update', {
                    method: 'post',
                    responseType: 'json',
                    headers: {
                        'content-type': 'application/json'
                    }
                }).then(response => {
                    if(!response.ok){ throw response }
                    this.action = null;
                    alert('Received The Order');

                }).catch(error => console.log(error))
            },
            updateTrackingNo(){
                fetch('https://'+ this.serverurl +'/api/v1/order/attachment/'+ this.orderId +'/tracking-number', {
                    method: 'post',
                    body: JSON.stringify({
                        type: 1,
                        courier: this.warehouseReceive.courier,
                        tracking_number: this.warehouseReceive.tracking_number
                    }),
                    headers: {
                        'content-type': 'application/json'
                    }
                }).then(response => {
                    if(!response.ok){ throw response }
                    alert('ok');
                    this.warehouseReceive = {
                        courier: '',
                        tracking_number: '',
                    }
                    this.action = null;

                }).catch(error => console.log(error))
            },
            scannedData(order_id)  {
                this.orderId = order_id;
                this.scannerActive = false;
            },
            onSubmitMeasure(){
                fetch('https://'+ this.serverurl +'/api/v1/order/attachment/'+ this.orderId +'/packing-list', {
                    method: 'post',
                    body: JSON.stringify({
                        reference_no: '4234123',
                        confirmed: 1,
                        packages: [{
                            description: this.packages.description,
                            type: this.packages.type,
                            quantity: this.packages.quantity,
                            width: this.packages.width,
                            height: this.packages.height,
                            length: this.packages.length
                        }]
                    }),
                    headers: {
                        'content-type': 'application/json'
                    }
                }).then(response => {
                    if(!response.ok){ throw response }
                    this.packages = {
                        description: '',
                        type:0,
                        quantity: 1,
                        width: 0.0,
                        height: 0.0,
                        length: 0.0
                    }
                    this.action = null;
                    alert('Packing List Complete');

                }).catch(error => console.log(error))
            },
            openCamera() {
                this.cameraSettings.show = !this.cameraSettings.show;
            },
            async takePic(){
                if (this.$refs.camera) {
                    console.log("Camera get image");

                    const options = {
                        fixOrientation: true,
                        base64: true,
                        width: 300,
                        height: 300
                    };
                    this.cameraSettings.show = false;
                    const data = await this.$refs.camera.takePictureAsync(options);
                    if (!!data) {
                        this.previewImage = {
                            uri: data.uri
                        };
                        console.log(this.previewImage);
                        this.cameraSettings.show = false;
                        this.preview = true;
                    } else {
                        alert("failed get picture");
                    }
                }

            }
        }

    };


</script>

<style>
    .container {
        width: 100%;
        min-height: 100%;
        align-content: center;
    }
    .icon-container {
        flex-direction: row;
        justify-content: center;
        align-items: center;
        padding-bottom: 50px;
    }
    .qr-container {
        width: 100%;
        height: 40%;
        min-height: 300px;
        align-items: center;
        justify-content: center;
    }
    .qr-detector {
        width: 200px;
        height: 200px;
    }
    .qr-detector-inner {
        flex-direction: row;
        justify-content: space-between;
    }
    .detector-corner {
        border-left-width: 4px;
        border-top-width: 4px;
        border-color: rgba(255, 255, 255, 0.7);
        width: 50px;
        height: 50px;
    }
    .corner-top-right {
        transform: scaleX(-1);
    }
    .corner-bottom-left {
        transform: scaleY(-1);
    }
    .corner-bottom-right {
        transform: scale(-1);
    }
    .btn-container {
        flex-direction: row;
        justify-content: center;
        align-items: stretch;
    }
    .scan-btn {
        flex: 1;
        height: 150px;
        justify-content: center;
        align-items: center;
    }
    .btn-text {
        font-size: 14px;
        font-weight: 700;
        text-transform: uppercase;
        justify-content: center;
    }
    .box {
        flex-direction: row;
        justify-content: center;
        align-items: center;
        width: 30%;
        padding-top: 10px;
        padding-bottom: 10px;
        margin-left: 20px;
        margin-bottom: 20px;
        background-color: #dfdfdf;
    }
    .thumbnail {
        flex-direction: row;
        justify-content: center;
        align-items: center;
        width: 20%;
        padding-top: 10px;
        padding-bottom: 10px;
        margin-left: 20px;
        margin-bottom: 20px;
        background-color: #dfdfdf;
    }
    .preview {
        flex:1;
    }
    .b-a {
        border-width: 1px;
        border-style: solid;
    }
    .b-info {
        flex: 1;
        height: 150px;
        justify-content: center;
        align-items: center;
        border-color: #4d6286;
    }
    .b-grey {
        flex: 1;
        height: 150px;
        justify-content: center;
        align-items: center;
        border-color: #ececec;
    }

</style>

