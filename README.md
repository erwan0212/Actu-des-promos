# Actu-des-promos import React, { useState, useEffect } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Search, Bell, Heart, Home, Grid } from "lucide-react";

const mockFetchPromotions = () => { return Promise.resolve([ { id: 1, title: "Google Chromecast", price: "29€", oldPrice: "49€", image: "https://via.placeholder.com/150", category: "High-tech", }, { id: 2, title: "Lessive Ariel 30 lavages", price: "5,99€", oldPrice: "8,99€", image: "https://via.placeholder.com/150", category: "Maison", }, { id: 3, title: "Apple AirPods Pro", price: "199€", oldPrice: "279€", image: "https://via.placeholder.com/150", category: "High-tech", }, { id: 4, title: "Nike Air Max 270", price: "89€", oldPrice: "139€", image: "https://via.placeholder.com/150", category: "Mode", }, ]); };

const categories = ["Toutes", "High-tech", "Maison", "Mode"];

export default function ActuPromotionsApp() { const [search, setSearch] = useState(""); const [selectedCategory, setSelectedCategory] = useState("Toutes"); const [promotions, setPromotions] = useState([]); const [favorites, setFavorites] = useState([]); const [notifications, setNotifications] = useState([]);

useEffect(() => { mockFetchPromotions().then((data) => setPromotions(data)); }, []);

const toggleFavorite = (id) => { setFavorites((prev) => prev.includes(id) ? prev.filter((fav) => fav !== id) : [...prev, id] ); setNotifications((prev) => [ ...prev, { id: Date.now(), message: "Promotion ajoutée aux favoris" }, ]); };

const filteredPromotions = promotions.filter((promo) => { const matchesCategory = selectedCategory === "Toutes" || promo.category === selectedCategory; const matchesSearch = promo.title.toLowerCase().includes(search.toLowerCase()); return matchesCategory && matchesSearch; });

return ( <div className="p-4 bg-gray-100 min-h-screen pb-20"> <div className="mb-4 flex items-center gap-2"> <Input placeholder="Rechercher une promotion..." className="flex-1" value={search} onChange={(e) => setSearch(e.target.value)} /> <Button variant="outline"> <Search className="w-5 h-5" /> </Button> </div>

<div className="mb-4 flex gap-2 overflow-x-auto">
    {categories.map((cat) => (
      <Button
        key={cat}
        variant={selectedCategory === cat ? "default" : "outline"}
        onClick={() => setSelectedCategory(cat)}
        className="whitespace-nowrap"
      >
        {cat}
      </Button>
    ))}
  </div>

  <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
    {filteredPromotions.map((promo) => (
      <Card key={promo.id} className="rounded-2xl shadow-md relative">
        <CardContent className="p-4">
          <img
            src={promo.image}
            alt={promo.title}
            className="w-full h-40 object-cover rounded-xl mb-4"
          />
          <h3 className="text-lg font-semibold mb-1">{promo.title}</h3>
          <div className="flex items-center gap-2">
            <span className="text-red-600 font-bold text-xl">{promo.price}</span>
            <span className="line-through text-gray-500">{promo.oldPrice}</span>
            <span className="ml-auto bg-orange-500 text-white px-2 py-1 text-xs rounded">HOT</span>
          </div>
          <Button
            onClick={() => toggleFavorite(promo.id)}
            variant="ghost"
            size="icon"
            className="absolute top-2 right-2"
          >
            <Heart
              className={`w-5 h-5 ${favorites.includes(promo.id) ? "fill-red-500 text-red-500" : "text-gray-400"}`}
            />
          </Button>
        </CardContent>
      </Card>
    ))}
  </div>

  {notifications.length > 0 && (
    <div className="fixed top-4 left-1/2 transform -translate-x-1/2 bg-green-500 text-white px-4 py-2 rounded-xl shadow-lg">
      {notifications[notifications.length - 1].message}
    </div>
  )}

  <div className="fixed bottom-0 left-0 right-0 flex justify-around bg-white border-t p-2 shadow z-10">
    <Button variant="ghost">
      <Home className="w-5 h-5 mr-1" /> Accueil
    </Button>
    <Button variant="ghost">
      <Grid className="w-5 h-5 mr-1" /> Catégories
    </Button>
    <Button variant="ghost">
      <Heart className="w-5 h-5 mr-1" /> Favoris
    </Button>
    <Button variant="ghost">
      <Bell className="w-5 h-5 mr-1" /> Alertes
    </Button>
  </div>
</div>

); }

