"# indoorAirQualityMonitoringSystem" 


interval:
  - interval: 500ms
    then:
      - lambda: |-
          // Get current PM2.5 value from the label text
          const char* label_text = lv_label_get_text(id(pm25_value_label));
          std::string text(label_text);
          
          // Extract numeric value from the label text
          float pm25 = 0.0;
          size_t pos = text.find("PM2.5:");
          if (pos != std::string::npos) {
            std::string value_str = text.substr(pos + 6); // Skip "PM2.5:"
            pos = value_str.find("mg/m3");
            if (pos != std::string::npos) {
              value_str = value_str.substr(0, pos);
              // Trim whitespace
              while (!value_str.empty() && isspace(value_str.front())) value_str.erase(0, 1);
              while (!value_str.empty() && isspace(value_str.back())) value_str.pop_back();
              
              // Parse the value
              char* end;
              pm25 = strtof(value_str.c_str(), &end);
              
              // Debug the extracted value
              ESP_LOGD("AirQuality", "Extracted PM2.5 value: %.2f from text: '%s'", 
                      pm25, value_str.c_str());
            }
          }
          
          // For testing, use dynamic value changes
          // static float test_value = 5.0;
          // test_value += 10.0;  // Increase by 10 each interval
          // if (test_value > 300) test_value = 5.0;  // Reset when too high
          // pm25 = test_value;  // Override with test value that will change
          
          ESP_LOGD("AirQuality", "Using PM2.5 value: %.2f", pm25);
          
          // Update image based on value
          if (pm25 < 12.0) {
            lv_img_set_src(id(air_quality_icon), id(GoodAir));
            ESP_LOGD("AirQuality", "Setting image to GoodAir");
          } else if (pm25 < 35.4) {
            lv_img_set_src(id(air_quality_icon), id(Moderate));
            ESP_LOGD("AirQuality", "Setting image to Moderate");
          } else if (pm25 < 55.5) {
            lv_img_set_src(id(air_quality_icon), id(UnhealthySG));
            ESP_LOGD("AirQuality", "Setting image to UnhealthySG");
          } else if (pm25 < 150.4) {
            lv_img_set_src(id(air_quality_icon), id(Unhealthy));
            ESP_LOGD("AirQuality", "Setting image to Unhealthy");
          } else if (pm25 < 250.4) {
            lv_img_set_src(id(air_quality_icon), id(VeryUnhealthy));
            ESP_LOGD("AirQuality", "Setting image to VeryUnhealthy");
          } else {
            lv_img_set_src(id(air_quality_icon), id(Hazardous));
            ESP_LOGD("AirQuality", "Setting image to Hazardous");
          }


Air Quality

Label - 
Image -
Range -
Text_colour - 
bg and border color - 

Good
Label - Good 
Image - GoodAir
Range - 0.00 - 9.00
Text_colour - 0x00b016
bg and border color - 0x68D36C


Moderate
Label - Moderate
Image - Moderate
Range - 9.01 - 35.40
Text_colour - 0xfcbf01
bg and border color - 0xfec81d

Unhealthy for Sensitive Groups
Label - Unhealthy for Sensitive Groups
Image - UnhealthySG
Range - 35.41 - 55.40
Text_colour - 
bg and border color - 


Unhealthy
Label - Unhealthy for All
Image - Unhealthy
Range - 55.41 - 125.40
Text_colour - 0xe70808
bg and border color - 0xf3624e


Very Unhealthy
Label - Very Unhealthy
Image - VeryUnhealthy
Range - 125.41 - 225.40
Text_colour - 0xaa4ef3
bg and border color - 0xc27ff7


Hazardous
Label - Hazardous
Image - Hazardous
Range -225.41 on words
Text_colour - 0x898989
bg and border color - 0xD9D8D8


{
  "room1":{
    "pm":{
      "pm1":{
        "raw":30,
        "calibrated":null
      },
      "pm2_5":{
        "raw":50,
        "calibrated":null
      },
      "pm10":{
        "raw":53,
        "calibrated":null
      }
    },
    "tvoc":{
      "raw":0.00,
      "calibrated":null
    },
    "hcho":{
      "raw":0.02,
      "calibrated":null
    },
    "co2":{
      "raw":520,
      "calibrated":null
    },
    "temp":{
      "raw":33.30,
      "calibrated":null
    },
    "humidity":{
      "raw":44.80,
      "calibrated":null
    }
  }
}
