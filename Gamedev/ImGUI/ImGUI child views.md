```
static float w = 200.0f;
static float scroll_y = 0.0f;

ImGui::BeginChild("child_1", {w, 0.0f}, true, ImGuiWindowFlags_NoScrollbar);
auto draw_list = ImGui::GetWindowDrawList();
auto cursor = ImGui::GetCursorScreenPos();
auto legend = ImGui::GetWindowSize();
ImGui::TextUnformatted("child_1");
for(auto i = 1; i < 14; i++) {
    draw_list->AddLine(cursor + ImVec2{5, i * 50.0f}, cursor + ImVec2{legend.x - 10, i * 50.0f}, 0xFFFFFFFF);
}
ImGui::SetCursorPos({0, 1500});
ImGui::SetScrollY(scroll_y);
ImGui::EndChild();

ImGui::SameLine();
cursor = ImGui::GetCursorPos();
ImGui::InvisibleButton("v_splitter", {8, ImGui::GetWindowSize().y - 10});
if(ImGui::IsItemActive()) {
    w += ImGui::GetIO().MouseDelta.x;
}
auto button = ImGui::GetItemRectSize();
draw_list->AddRect(cursor, cursor + button, 0xFFFFFFFF);

ImGui::SameLine();
ImGui::BeginChild("child_2", {0, 0}, false, ImGuiWindowFlags_HorizontalScrollbar);
draw_list = ImGui::GetWindowDrawList();
cursor = ImGui::GetCursorScreenPos();
ImGui::TextUnformatted("child_2");
for(auto i = 1; i < 14; i++) {
    draw_list->AddLine(cursor + ImVec2{5, i * 50.0f}, cursor + ImVec2{1495, i * 50.0f}, 0xFFFFFFFF);
}
draw_list->AddLine(cursor + ImVec2{50, 50}, cursor + ImVec2{1000, 1000}, 0xFFFFFFFF);
ImGui::SetCursorPos({1500, 1500});
scroll_y = ImGui::GetScrollY();
ImGui::EndChild();
```

#gamedev #imgui