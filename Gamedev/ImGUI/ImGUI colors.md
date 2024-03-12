```
float rgb[] {red/255, green/255, blue/255};

ImGui::ColorPicker3("##MyColor##", (float*)&rgb, ImGuiColorEditFlags_DisplayRGB | ImGuiColorEditFlags_NoAlpha);

current_shape.shape().setFillColor(sf::Color(rgb[0]*255, rgb[1]*255, rgb[2]*255));

```

