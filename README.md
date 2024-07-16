class character_minimap_component : public uobject {
public:
    void set_observer_status(bool status) {
        if(read<bool>(std::uintptr_t(this) + 0x530) != status)
            write<bool>(std::uintptr_t(this) + 0x530, status);
    }
 
    void set_visible_status(bool status) {
        if (read<bool>(std::uintptr_t(this) + 0x501) != status)
            write<bool>(std::uintptr_t(this) + 0x501, status);
    }
};
 
// Actor
character_minimap_component* get_portrait_minimap_component() {
    return read<character_minimap_component>(std::uintptr_t(this) + 0x1090);
}
 
character_minimap_component get_character_minimap_component() {
    return read<character_minimap_component*>(std::uintptr_t(this) + 0x1098);
}
 
for (int i = 0; i < actors.size(); i++) {
    if (auto actor = actors[i]; actor != local_pawn) {
        if (auto character_minimap = actor->get_character_minimap_component(); auto portrait_minimap = actor->get_portrait_minimap_component()) {
            character_minimap->set_observer_status(actor->is_dormant());
            character_minimap->set_visible_status(actor->is_dormant());
 
            portrait_minimap->set_observer_status(actor->is_dormant());
            portrait_minimap->set_visible_status(actor->is_dormant());
        }
    }
} 
