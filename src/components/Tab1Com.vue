<template>
  <div class="main">
    <el-button class="btn" type="primary" @click="changeTranslationStatus">切换画布平移状态</el-button>
    <el-button class="btn" type="primary" @click="zoomIn">缩小</el-button>
    <el-button class="btn" type="primary" @click="zoomOut">放大</el-button>
    <el-button class="btn" type="primary" @click="zoomDefault">还原</el-button>
    <el-button class="btn" type="primary" @click="zoomCenter">内容居中</el-button>
    <el-button class="btn" type="primary" @click="exportToSVG">导出SVG</el-button>
    <el-button class="btn" type="primary" @click="exportToPNG">导出PNG</el-button>
    <el-button class="btn" type="primary" @click="exportToJPEG">导出JPEG</el-button>
    <el-button class="btn" type="primary" @click="exportToJSON">导出JSON</el-button>
    <el-button class="btn" type="primary" @click="addNodeAndEdge">增加节点</el-button>
    <el-button class="btn" type="primary" @click="addCustomNode">自定义节点</el-button>
    <div class="container" ref="graph-container"></div>
  </div>
</template>

<script>
import { DataUri, Graph, Shape } from '@antv/x6'
export default {
  name: 'Tab1Com',
  data () {
    return {
      graph: null,
      defaultZoomLevel: null,
      jsonData: {
        nodes: [
          {
            id: 'node1',
            shape: 'rect',
            x: 50,
            y: 50,
            width: 80,
            height: 40,
            label: '矩形'
          },
          {
            id: 'node2',
            shape: 'ellipse',
            x: 250,
            y: 50,
            width: 80,
            height: 40,
            label: '椭圆'
          },
          {
            id: 'node3',
            shape: 'circle',
            x: 450,
            y: 50,
            width: 40,
            height: 40,
            label: '圆形'
          },
          {
            id: 'node4',
            shape: 'polygon',
            x: 650,
            y: 50,
            width: 80,
            height: 40,
            label: '多边形',
            // 第一种定义节点
            // points: [
            //   [0, 10],
            //   [10, 0],
            //   [20, 10],
            //   [10, 20]
            // ],
            // 第二种定义节点
            points: [
              { x: 0, y: 10 },
              { x: 10, y: 0 },
              { x: 20, y: 10 },
              { x: 10, y: 20 }
            ],
            attrs: {
              body: {
                fill: '#efdbff', // 填充色
                stroke: '#9254de' // 边框色
                // 第三种定义节点
                // refPoints: '0,10 10,0 20,10 10,20'
              }
            }
          },
          {
            id: 'node5',
            shape: 'polygon',
            x: 850,
            y: 30,
            width: 80,
            height: 80,
            label: '多边形',
            attrs: {
              body: {
                fill: '#ffd591',
                stroke: '#ffa940',
                strokeWidth: 2,
                // fillRule: 'evenodd',
                fillRule: 'nonzero',
                refPoints: '100,10 40,198 190,78 10,78 160,198'
              }
            }
          },
          {
            id: 'node6',
            shape: 'polyline', // 折线
            x: 1050,
            y: 30,
            width: 80,
            height: 80,
            label: '折线',
            attrs: {
              body: {
                fill: '#efdbff',
                stroke: '#9254de',
                strokeWidth: 2,
                refPoints: '0,0 0,10 10,10 10,0'
              }
            }
          },
          {
            id: 'node7',
            shape: 'polyline', // 折线
            x: 1250,
            y: 30,
            width: 80,
            height: 80,
            // label: '折线',
            attrs: {
              body: {
                fill: 'none',
                stroke: '#9254de',
                strokeWidth: 5,
                refPoints: '0,40 40,40 40,80 80,80 80,120 120,120 120,160'
              }
            }
          },
          {
            id: 'node8',
            shape: 'path', // 折线
            x: 1450,
            y: 30,
            width: 80,
            height: 80,
            label: '路径',
            // 使用 path 属性指定路径的 pathData，相当于指定路径的 refD 属性
            // 在线画SVG
            path: 'M 0 5 10 0 C 20 0 20 20 10 20 L 0 15 Z',
            attrs: {
              body: {
                fill: '#efdbff',
                stroke: '#9254de',
                strokeWidth: 3
                // 指定 refD 属性，pathData 随图形大小自动缩放
                // refD: 'M 0 5 10 0 C 20 0 20 20 10 20 L 0 15 Z',
              }
            }
          },
          {
            id: 'node9',
            shape: 'path', // 折线
            x: 1650,
            y: 30,
            width: 100,
            height: 80,
            // label: '路径',
            // 使用 path 属性指定路径的 pathData，相当于指定路径的 refD 属性
            // 在线画SVG
            // path: 'm220.76996,75.87745c-5.96468,2.54667 -11.40757,5.94182 -16.89394,8.96796c-1.86974,1.03047 -3.99384,2.66842 -5.72661,3.16922c-4.41562,1.27145 -11.32495,-0.95265 -16.95373,-1.18485c-14.06868,-0.57611 -23.17713,0.61878 -34.74232,2.63202c-5.41245,0.9401 -11.01623,1.82121 -15.76666,3.42904c-4.05798,1.37438 -8.71058,4.4457 -13.25122,7.02877c-7.17566,4.08296 -12.06306,7.92745 -17.65814,12.8501c-9.35194,8.22241 -15.23944,18.57353 -22.08246,29.94634c-2.98832,4.96407 -7.29197,12.3606 -7.50612,18.23464c-3.9558,6.92209 -9.96613,11.14814 -11.47388,21.28086c2.1078,5.92551 7.57026,0.65644 10.86187,-1.31664c2.72199,-1.6367 5.65052,-3.5922 8.0627,-4.37667c12.53702,-4.08045 30.00819,1.75217 42.21474,-1.98688c4.32648,-1.32417 8.07465,-3.99761 12.37287,-5.12975c4.0819,-1.07816 7.20392,0.8309 10.11397,2.28184c1.54579,0.77316 3.1742,1.81995 4.57868,2.65211c9.09431,5.42596 18.18428,11.17826 30.39409,13.34463c9.79981,1.73586 18.31364,-1.57645 26.27958,-3.36628c2.02192,-0.45562 4.63412,-0.28994 6.16796,-2.2831c-6.35819,-1.65804 -11.63476,-4.48586 -18.2745,-7.50447c-8.72145,-3.96122 -15.49599,-6.64093 -18.79194,-14.59349c3.90362,-1.17983 8.42469,-0.305 12.4881,0.51712c4.43736,0.89366 8.84103,1.73335 13.09903,2.54165c17.97882,3.41774 31.5192,9.15498 45.20523,16.59794c6.64844,3.61103 12.77508,8.64288 18.41038,12.86642c11.54128,8.66045 21.39327,19.41447 30.68542,31.12993c4.20582,5.30421 7.96269,11.9464 12.52397,18.27732c1.99366,2.76758 3.99819,5.85522 6.12882,9.27296c2.03606,3.24704 5.14286,7.22708 5.57334,10.06746c0.48483,3.13659 -0.9577,6.57567 -1.68385,10.00345c-1.25555,5.95437 -2.18172,12.34177 -1.76103,18.45555c0.55005,7.90737 2.08063,15.6089 5.28962,21.59967c1.39252,-1.4296 1.96105,-3.94615 2.67851,-6.25059c3.51771,-5.19125 8.19749,-10.37623 8.31381,-19.01534c9.60849,5.73347 28.27216,5.25275 39.47753,5.10339c3.36879,-0.04518 6.80062,0.3665 9.5911,-1.14594c-5.88532,-3.93862 -10.20528,-8.92278 -15.32097,-14.0111c-7.73984,-7.68772 -14.14151,-14.16423 -25.55126,-18.27732c-3.69817,-1.33296 -7.93225,-2.05466 -9.95852,-4.52477c-1.55775,-1.89024 -2.03171,-6.07738 -2.822,-10.11014c-2.12519,-10.90338 -4.88849,-22.47199 -7.35394,-31.6483c-7.3985,-27.58291 -18.69084,-52.09701 -33.71505,-71.13494c-7.61374,-9.65452 -16.70588,-17.51671 -26.50895,-26.25874c-1.67515,-3.03241 -4.81348,-7.12291 -5.46137,-11.74684c-0.6631,-4.76074 0.53266,-8.93909 3.08072,-12.13718c5.57442,-4.69673 14.33284,-5.21635 18.12014,-12.24638c-4.95154,-4.1156 -11.09558,-5.32178 -17.1244,-5.99454c-13.02402,-1.4547 -25.22623,-0.32508 -35.32933,3.99385z',
            attrs: {
              body: {
                fill: '#efdbff',
                stroke: '#9254de',
                strokeWidth: 3,
                // 指定 refD 属性，pathData 随图形大小自动缩放
                refD: 'm220.76996,75.87745c-5.96468,2.54667 -11.40757,5.94182 -16.89394,8.96796c-1.86974,1.03047 -3.99384,2.66842 -5.72661,3.16922c-4.41562,1.27145 -11.32495,-0.95265 -16.95373,-1.18485c-14.06868,-0.57611 -23.17713,0.61878 -34.74232,2.63202c-5.41245,0.9401 -11.01623,1.82121 -15.76666,3.42904c-4.05798,1.37438 -8.71058,4.4457 -13.25122,7.02877c-7.17566,4.08296 -12.06306,7.92745 -17.65814,12.8501c-9.35194,8.22241 -15.23944,18.57353 -22.08246,29.94634c-2.98832,4.96407 -7.29197,12.3606 -7.50612,18.23464c-3.9558,6.92209 -9.96613,11.14814 -11.47388,21.28086c2.1078,5.92551 7.57026,0.65644 10.86187,-1.31664c2.72199,-1.6367 5.65052,-3.5922 8.0627,-4.37667c12.53702,-4.08045 30.00819,1.75217 42.21474,-1.98688c4.32648,-1.32417 8.07465,-3.99761 12.37287,-5.12975c4.0819,-1.07816 7.20392,0.8309 10.11397,2.28184c1.54579,0.77316 3.1742,1.81995 4.57868,2.65211c9.09431,5.42596 18.18428,11.17826 30.39409,13.34463c9.79981,1.73586 18.31364,-1.57645 26.27958,-3.36628c2.02192,-0.45562 4.63412,-0.28994 6.16796,-2.2831c-6.35819,-1.65804 -11.63476,-4.48586 -18.2745,-7.50447c-8.72145,-3.96122 -15.49599,-6.64093 -18.79194,-14.59349c3.90362,-1.17983 8.42469,-0.305 12.4881,0.51712c4.43736,0.89366 8.84103,1.73335 13.09903,2.54165c17.97882,3.41774 31.5192,9.15498 45.20523,16.59794c6.64844,3.61103 12.77508,8.64288 18.41038,12.86642c11.54128,8.66045 21.39327,19.41447 30.68542,31.12993c4.20582,5.30421 7.96269,11.9464 12.52397,18.27732c1.99366,2.76758 3.99819,5.85522 6.12882,9.27296c2.03606,3.24704 5.14286,7.22708 5.57334,10.06746c0.48483,3.13659 -0.9577,6.57567 -1.68385,10.00345c-1.25555,5.95437 -2.18172,12.34177 -1.76103,18.45555c0.55005,7.90737 2.08063,15.6089 5.28962,21.59967c1.39252,-1.4296 1.96105,-3.94615 2.67851,-6.25059c3.51771,-5.19125 8.19749,-10.37623 8.31381,-19.01534c9.60849,5.73347 28.27216,5.25275 39.47753,5.10339c3.36879,-0.04518 6.80062,0.3665 9.5911,-1.14594c-5.88532,-3.93862 -10.20528,-8.92278 -15.32097,-14.0111c-7.73984,-7.68772 -14.14151,-14.16423 -25.55126,-18.27732c-3.69817,-1.33296 -7.93225,-2.05466 -9.95852,-4.52477c-1.55775,-1.89024 -2.03171,-6.07738 -2.822,-10.11014c-2.12519,-10.90338 -4.88849,-22.47199 -7.35394,-31.6483c-7.3985,-27.58291 -18.69084,-52.09701 -33.71505,-71.13494c-7.61374,-9.65452 -16.70588,-17.51671 -26.50895,-26.25874c-1.67515,-3.03241 -4.81348,-7.12291 -5.46137,-11.74684c-0.6631,-4.76074 0.53266,-8.93909 3.08072,-12.13718c5.57442,-4.69673 14.33284,-5.21635 18.12014,-12.24638c-4.95154,-4.1156 -11.09558,-5.32178 -17.1244,-5.99454c-13.02402,-1.4547 -25.22623,-0.32508 -35.32933,3.99385z'
              }
            }
          },
          {
            id: 'node10',
            shape: 'image',
            x: 1850,
            y: 50,
            width: 80,
            height: 40,
            // label: '图片',
            imageUrl: 'https://gw.alipayobjects.com/os/s/prod/antv/assets/image/logo-with-text-73b8a.svg'
            // imageUrl: '@/src/assets/logo.png'
          },
          {
            id: 'node11',
            shape: 'image-bordered', // 带边框的图片
            x: 1850,
            y: 250,
            width: 80,
            height: 40,
            // label: '图片',
            imageUrl: 'https://gw.alipayobjects.com/os/s/prod/antv/assets/image/logo-with-text-73b8a.svg'
          },
          {
            id: 'node12',
            shape: 'image-embedded', // 内嵌入矩形的图片
            x: 1650,
            y: 250,
            width: 80,
            height: 40,
            // label: '图片',
            imageUrl: 'https://gw.alipayobjects.com/os/s/prod/antv/assets/image/logo-with-text-73b8a.svg'
          },
          {
            id: 'node13',
            shape: 'image-inscribed', // 内嵌入椭圆的图片
            x: 1450,
            y: 250,
            width: 80,
            height: 40,
            // label: '图片',
            imageUrl: 'https://gw.alipayobjects.com/os/s/prod/antv/assets/image/logo-with-text-73b8a.svg'
          },
          {
            id: 'node14',
            shape: 'text-block',
            x: 1250,
            y: 230,
            width: 110,
            height: 80,
            text: '晚星就像你的眼睛杀人又放火',
            label: '文本节点',
            attrs: {
              body: {
                fill: '#efdbff',
                stroke: '#9254de',
                strokeWidth: 3,
                rx: 10,
                ry: 10
              },
              label: {
                refX: 0.5,
                refY: '100%',
                refY2: 10,
                textAnchor: 'middle',
                textVerticalAnchor: 'top'
              }
            }
          },
          {
            id: 'node15',
            shape: 'cylinder',
            x: 1050,
            y: 200,
            width: 80,
            height: 140,
            label: '圆柱',
            attrs: {
              top: {
                fill: '#efdbff',
                fillOpacity: 0.5,
                stroke: '#9254de',
                strokeWidth: 3
              },
              body: {
                fill: '#9254de',
                fillOpacity: 0.6,
                stroke: '#9254de',
                strokeWidth: 3
              }
            }
          },
          {
            id: 'node16',
            shape: 'html',
            x: 850,
            y: 250,
            width: 80,
            height: 40,
            label: '嵌入Html',
            attrs: {
              label: {
                refX: 0.5,
                refY: '100%',
                refY2: 10,
                textAnchor: 'middle',
                textVerticalAnchor: 'top'
              }
            },
            html () {
              const wrap = document.createElement('div')
              wrap.style.width = '100%'
              wrap.style.height = '100%'
              wrap.style.display = 'flex'
              wrap.style.alignItems = 'center'
              wrap.style.justifyContent = 'center'
              wrap.style.border = '2px solid #9254de'
              wrap.style.background = '#efdbff'
              wrap.style.borderRadius = '4px'
              wrap.innerText = 'Html'
              return wrap
            }
          }
        ],
        edges: [
          {
            source: 'node1',
            target: 'node2',
            shape: 'edge',
            label: '默认边'
          },
          {
            source: 'node2',
            target: 'node3',
            shape: 'double-edge',
            label: '双线边'
          },
          {
            source: 'node3',
            target: 'node4',
            shape: 'shadow-edge',
            label: '阴影边'
          },
          {
            source: 'node4',
            target: 'node5'
            // shape: 'double-edge',
            // label: '双线边'
          },
          {
            source: 'node5',
            target: 'node6'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node6',
            target: 'node7'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node7',
            target: 'node8'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node8',
            target: 'node9'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node9',
            target: 'node10'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node10',
            target: 'node11'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node11',
            target: 'node12'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node12',
            target: 'node13'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node13',
            target: 'node14'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node14',
            target: 'node15'
            // shape: 'shadow-edge',
            // label: '阴影边'
          },
          {
            source: 'node15',
            target: 'node16'
            // shape: 'shadow-edge',
            // label: '阴影边'
          }
        ]
      }
    }
  },
  mounted () {
    this.initGraph()
  },
  destroyed () {
    this.graph.dispose()
  },
  methods: {
    initGraph () {
      this.graph = new Graph({
        container: this.$refs['graph-container'],
        height: 1000,
        background: {
          color: '#fffbe6'
        },
        grid: {
          size: 10,
          visible: true
        },
        panning: {
          enabled: true
          // modifiers: 'shift',
          // eventTypes: ['leftMouseDown', 'mouseWheel']
        }
      })
      this.graph.fromJSON(this.jsonData)
      // this.graph.zoomCenter()
      this.defaultZoomLevel = this.graph.zoom()
    },
    changeTranslationStatus () {
      this.graph.togglePanning()
    },
    zoomIn () {
      this.graph.zoom(-0.1)
    },
    zoomOut () {
      this.graph.zoom(0.1)
    },
    zoomDefault () {
      this.graph.zoom(this.defaultZoomLevel - this.graph.zoom())
    },
    zoomCenter () {
      // this.graph.zoomCenter()
      this.graph.centerContent()
    },
    exportToSVG () {
      this.graph.zoom(this.defaultZoomLevel - this.graph.zoom())
      this.graph.toSVG((dataUri) => {
        DataUri.downloadDataUri(DataUri.svgToDataUrl(dataUri), 'chart.svg')
      }, {
        // preserveDimensions 用来控制导出 svg 的尺寸, 如果不设置，width 和 height 默认为 100%；如果设置为 true, width 和 height 会自动计算为图形区域的实际大小。还可以通过以下方式自定义尺寸：
        // preserveDimensions: true,
        // preserveDimensions: {
        //   width: 1920,
        //   height: 1080
        // },
        // 是否将 image 元素的 xlink:href 链接转化为 dataUri 格式
        serializeImages: true,
        // 是否复制外部样式表中的样式，默认是 true。开启 copyStyles 后，在导出过程中因为需要禁用所有样式表，所以页面可能会出现短暂的样式丢失现象。
        copyStyles: false
      })
    },
    exportToPNG () {
      this.graph.zoom(this.defaultZoomLevel - this.graph.zoom())
      this.graph.toPNG((dataUri) => {
        DataUri.downloadDataUri(dataUri, 'chart.png')
      }, {
        padding: {
          top: 10,
          right: 10,
          bottom: 10,
          left: 10
        },
        copyStyles: false,
        quality: 1
      })
    },
    exportToJPEG () {
      this.graph.zoom(this.defaultZoomLevel - this.graph.zoom())
      this.graph.toPNG((dataUri) => {
        DataUri.downloadDataUri(dataUri, 'chart.jpeg')
      }, {
        padding: {
          top: 10,
          right: 10,
          bottom: 10,
          left: 10
        },
        copyStyles: false,
        quality: 1
      })
    },
    exportToJSON () {
      console.log(this.graph.toJSON())
    },
    addNodeAndEdge () {
      const rect = new Shape.Rect({
        id: 'node17',
        x: 40,
        y: 250,
        width: 100,
        height: 40,
        label: 'rect',
        zIndex: 2
      })

      const circle = new Shape.Circle({
        id: 'node18',
        x: 280,
        y: 350,
        width: 60,
        height: 60,
        label: 'circle',
        zIndex: 2
      })

      const edge = new Shape.Edge({
        id: 'edge16',
        source: rect,
        target: circle,
        zIndex: 1
      })

      this.graph.addNode(rect)
      this.graph.addNode(circle)
      this.graph.addEdge(edge)
    },
    addCustomNode () {
      // 自定义节点，第一种便捷方法
      // Shape.Rect.define({
      //   shape: 'custom-rect',
      //   width: 300, // 默认宽度
      //   height: 40, // 默认高度
      //   attrs: {
      //     body: {
      //       rx: 10, // 圆角矩形
      //       ry: 10,
      //       strokeWidth: 1,
      //       fill: '#5755a1',
      //       stroke: '#5755a1'
      //     },
      //     label: {
      //       fill: '#fff',
      //       fontSize: 18,
      //       refX: 10, // x 轴偏移，类似 css 中的 margin-left
      //       textAnchor: 'left' // 左对齐
      //     }
      //   }
      // })
      // 自定义节点，第二种便捷方法
      Graph.registerNode('custom-rect', {
        inherit: 'rect', // 继承自 Shape.Rect
        width: 300, // 默认宽度
        height: 40, // 默认高度
        attrs: {
          body: {
            rx: 10, // 圆角矩形
            ry: 10,
            strokeWidth: 1,
            fill: '#5755a1',
            stroke: '#5755a1'
          },
          label: {
            fill: '#fff',
            fontSize: 18,
            refX: 10, // x 轴偏移，类似 css 中的 margin-left
            textAnchor: 'left' // 左对齐
          }
        }
      })
      this.graph.addNode({
        x: 100,
        y: 260,
        shape: 'custom-rect',
        label: 'My Custom Rect' // label 继承于基类的自定义选项
      })
    }
  }
}
</script>

<style scoped lang="less">
.btn {
  //padding: 10px;
  margin: 10px;
}
</style>
