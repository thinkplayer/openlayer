<template>
  <div class="container">
    <el-tabs tab-position="left">
      <el-tab-pane label="点标">
        <el-radio-group v-model="curRadio" @change="handleChangeRadio">
          <el-radio-button
            v-for="(it_point, in_point) in pointArr"
            :key="in_point"
            :label="it_point.value"
          >{{ it_point.label }}
          </el-radio-button
          >
        </el-radio-group>
      </el-tab-pane>
      <el-tab-pane label="线标">
        <el-radio-group v-model="curRadio" @change="handleChangeRadio">
          <el-radio-button
            v-for="(it_line, in_line) in lineArr"
            :key="in_line"
            :label="it_line.value"
          >{{ it_line.label }}
          </el-radio-button
          >
        </el-radio-group>
      </el-tab-pane>
      <el-tab-pane label="面标">
        <el-radio-group v-model="curRadio" @change="handleChangeRadio">
          <el-radio-button
            v-for="(it_surface, in_surface) in surfaceArr"
            :key="in_surface"
            :label="it_surface.value"
          >{{ it_surface.label }}
          </el-radio-button
          >
        </el-radio-group>
      </el-tab-pane>
      <el-tab-pane label="箭头">
        <el-radio-group v-model="curRadio" @change="handleChangeRadio">
          <el-radio-button
            v-for="(it_arrow, in_arrow) in arrowArr"
            :key="in_arrow"
            :label="it_arrow.label"
          >{{ it_arrow.label }}
          </el-radio-button
          >
        </el-radio-group>
      </el-tab-pane>
      <el-tab-pane label="操作">
        <el-radio-group v-model="curRadio" @change="handleChangeRadio">
          <el-radio-button
            v-for="(it_opt, in_opt) in optArr"
            :key="in_opt"
            :label="it_opt.label"
          >{{ it_opt.label }}
          </el-radio-button
          >
        </el-radio-group>
      </el-tab-pane>
      <el-tab-pane label="添加属性">添加属性</el-tab-pane>
      <el-tab-pane label="样式修改">
        <el-row :gutter="10">
          <el-form ref="form">
            <el-col :span="12">
              <el-form-item label="填充颜色调整">
                <el-color-picker
                  v-model="styleObj.fillColor"
                  :predefine="styleObj.predefineColors"
                  show-alpha
                ></el-color-picker>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="边框颜色调整">
                <el-color-picker
                  v-model="styleObj.storkColor"
                  :predefine="styleObj.predefineColors"
                  show-alpha
                ></el-color-picker>
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item label="边框粗细调整"></el-form-item>
            </el-col>
            <el-col :span="16">
              <el-slider v-model="styleObj.borderWidth" @change="handleChangeBorderWidth"></el-slider>
            </el-col>

          </el-form>
        </el-row>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
export default {
  data() {
    return {
      curRadio: '',
      styleObj: {
        // 填充颜色
        fillColor: '',
        // 边框颜色
        storkColor: '',
        // 默认可选择颜色
        predefineColors: [
          '#ff4500',
          '#ff8c00',
          '#ffd700',
          '#90ee90',
          '#00ced1',
          '#1e90ff',
          '#c71585',
          'rgba(255, 69, 0, 0.68)',
          'rgb(255, 120, 0)',
          'hsv(51, 100, 98)',
          'hsva(120, 40, 94, 0.5)',
          'hsl(181, 100%, 37%)',
          'hsla(209, 100%, 56%, 0.73)',
          '#c7158577'
        ],
        borderWidth: 66
      },
      // 点
      pointArr: [{ label: '点', value: 'Point' }],
      // 线
      lineArr: [
        { label: '弧线', value: 'LineString' },
        { label: '曲线', value: 'LineString' },
        { label: '折线', value: 'LineString' },
        { label: '自由线', value: 'LineString' }
      ],
      // 面
      surfaceArr: [
        { label: '圆', value: 'Circle' },
        { label: '椭圆', value: 'Circle' },
        { label: '弓形', value: 'Circle' },
        { label: '扇形', value: 'Circle' },
        { label: '曲线面', value: 'Circle' },
        { label: '多边形', value: 'MultiPolygon' },
        { label: '矩形', value: 'Circle' },
        { label: '自由面', value: 'Circle' },
        { label: '聚集地', value: 'Circle' }
      ],
      // 箭头
      arrowArr: [
        { label: '钳击', value: 'Point' },
        { label: '直箭头', value: 'Point' },
        { label: '细直箭头', value: 'Point' },
        { label: '突击方向', value: 'Point' },
        { label: '进攻方向', value: 'Point' },
        { label: '进攻方向(尾)', value: 'Point' },
        { label: '分队战斗行动', value: 'Point' },
        { label: '分队战斗行动(尾)', value: 'Point' }
      ],
      // 操作
      optArr: [
        { label: '上移', value: 'Point' },
        { label: '下移', value: 'Point' },
        { label: '置顶', value: 'Point' },
        { label: '置地', value: 'Point' },
        { label: '全部清除', value: 'Point' },
        { label: '屏蔽点击', value: 'Point' },
        { label: '激活点击', value: 'Point' }
      ]
    }
  },

  methods: {
    handleChangeRadio(v) {
      this.$emit('chooseFeatureType', v)
    },
    handleChangeBorderWidth(v) {
      console.log('%c🌼v:', 'font-size:20px;border:3px inset #42b983', v)
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  padding: 20px;

  .el-radio-button {
    margin-bottom: 10px;
  }
}
</style>
