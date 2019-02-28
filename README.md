

    本插件是对于gauge.js的基本封装
https://github.com/bernii/gauge.js


参数


opts 请参照官网opts
maxValue  设置最大值 type:number required:true default:100
animationSpeed 指针动画速度 type:number required:false default:32
actualValue  当前值 type:number required:true default:50
style canvas的样式，参照vue style的设置


注意：
1）官方gauge的圆形仪表盘不是圆形的，这里我修改了源码，标注①
this.ctx.lineCap = "round";


2）官方gauge的圆形仪表盘绘画方式相反了，这里我修改了源码②、③


Gauge.prototype.render = function() {
      var displayedAngle, fillStyle, gauge, h, j, k, len, len1, max, min, radius, ref, ref1, scaleMutate, tmpRadius, w, zone;
      w = this.canvas.width / 2;
      h = (this.canvas.height * this.paddingTop + this.availableHeight) - ((this.radius + this.lineWidth / 2) * this.extraPadding);
      displayedAngle = this.getAngle(this.displayedValue);
      if (this.textField) {
        this.textField.render(this);
      }
      this.ctx.lineCap = "round";//①
      radius = this.radius * this.options.radiusScale;
      if (this.options.staticLabels) {
        this.renderStaticLabels(this.options.staticLabels, w, h, radius);
      }
      if (this.options.staticZones) {
        this.ctx.save();
        this.ctx.translate(w, h);
        this.ctx.lineWidth = this.lineWidth;
        ref = this.options.staticZones;
        for (j = 0, len = ref.length; j < len; j++) {
          zone = ref[j];
          min = zone.min;
          if (this.options.limitMin && min < this.minValue) {
            min = this.minValue;
          }
          max = zone.max;
          if (this.options.limitMax && max > this.maxValue) {
            max = this.maxValue;
          }
          tmpRadius = this.radius * this.options.radiusScale;
          if (zone.height) {
            this.ctx.lineWidth = this.lineWidth * zone.height;
            scaleMutate = (this.lineWidth / 2) * (zone.offset || 1 - zone.height);
            tmpRadius = (this.radius * this.options.radiusScale) + scaleMutate;
          }
          this.ctx.strokeStyle = zone.strokeStyle;
          this.ctx.beginPath();
          this.ctx.arc(0, 0, tmpRadius, this.getAngle(min), this.getAngle(max), false);
          this.ctx.stroke();
        }
      } else {
        if (this.options.customFillStyle !== void 0) {
          fillStyle = this.options.customFillStyle(this);
        } else if (this.percentColors !== null) {
          fillStyle = this.getColorForValue(this.displayedValue, this.options.generateGradient);
        } else if (this.options.colorStop !== void 0) {
          if (this.options.gradientType === 0) {
            fillStyle = this.ctx.createRadialGradient(w, h, 9, w, h, 70);
          } else {
            fillStyle = this.ctx.createLinearGradient(0, 0, w, 0);
          }
          fillStyle.addColorStop(0, this.options.colorStart);
          fillStyle.addColorStop(1, this.options.colorStop);
        } else {
          fillStyle = this.options.colorStart;
        }
        this.ctx.strokeStyle = this.options.strokeColor;
        this.ctx.beginPath();
        this.ctx.arc(w, h, radius, displayedAngle, (2 - this.options.angle) * Math.PI, false); //②
        this.ctx.lineWidth = this.lineWidth;
        this.ctx.stroke();
        this.ctx.strokeStyle = fillStyle;
        this.ctx.beginPath();
        this.ctx.arc(w, h, radius, (1 + this.options.angle) * Math.PI, displayedAngle, false);//③
        this.ctx.stroke();
        this.ctx.save();
        this.ctx.translate(w, h);
      }
      if (this.options.renderTicks) {
        this.renderTicks(this.options.renderTicks, w, h, radius);
      }
      this.ctx.restore();
      this.ctx.translate(w, h);
      ref1 = this.gp;
      for (k = 0, len1 = ref1.length; k < len1; k++) {
        gauge = ref1[k];
        gauge.update(true);
      }
      return this.ctx.translate(-w, -h);
    };






