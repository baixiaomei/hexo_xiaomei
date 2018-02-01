/**
 * 火星坐标系 (GCJ-02) 与百度坐标系 (BD-09) 的转换
 * 即谷歌、高德 转 百度
 * @param lng
 * @param lat
 * @returns {*[]}
 */
export const gcj02tobd09 = (gcjlng, gcjlat) => {
  var lng = +gcjlng
  var lat = +gcjlat
  const XPI = 3.14159265358979324 * 3000.0 / 180.0
  var z = Math.sqrt(lng * lng + lat * lat) + 0.0002 * Math.sin(lat * XPI)
  var theta = Math.atan2(lat, lng) + 0.000003 * Math.cos(lng * XPI)
  var bdLng = z * Math.cos(theta) + 0.0065
  var bdLat = z * Math.sin(theta) + 0.006
  return {bdLng, bdLat}
}



import { create58Script, gcj02tobd09 } from '../utils/util'


// 如果是58用户 高德坐标转百度坐标
        if (window.suyun || window.navigator.userAgent.indexOf('58SuYunDriverIOS') >= 0) {
          var location = gcj02tobd09(this.location.longitude, this.location.latitude)
        }