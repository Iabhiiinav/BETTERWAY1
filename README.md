//Filtering & Sorting Logic
const filteredProducts = useMemo(() => {
  return products
    .filter(p => p.title.toLowerCase().includes(searchTerm.toLowerCase()))
    .filter(p => !category || p.category === category)
    .sort((a, b) => sortOrder === 'low' ? a.price - b.price : b.price - a.price);
}, [products, searchTerm, category, sortOrder]);

//JavaScript
const addToCart = (product) => {
  setCart(prevCart => {
    const existingItem = prevCart.find(item => item.id === product.id);
    
    if (existingItem) {
      // Check stock limit (assuming 'stock' property exists from dummyjson)
      if (existingItem.quantity >= product.stock) {
        alert("Out of stock!");
        return prevCart;
      }
      return prevCart.map(item => 
        item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item
      );
    }
    return [...prevCart, { ...product, quantity: 1 }];
  });
};
