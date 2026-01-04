//JavaScript
// Inside ProductList.js
const filteredProducts = useMemo(() => {
  return products
    .filter(p => p.title.toLowerCase().includes(searchTerm.toLowerCase()))
    .filter(p => !category || p.category === category)
    .sort((a, b) => {
      if (sortOrder === 'low-high') return a.price - b.price;
      if (sortOrder === 'high-low') return b.price - a.price;
      return 0;
    });
}, [products, searchTerm, category, sortOrder]);

//State Management & Performance
const ProductCard = React.memo(({ product, onAddToCart }) => {
  return (
    <div className="card">
      <h3>{product.title}</h3>
      <p>${product.price}</p>
      <button 
        disabled={product.stock === 0} 
        onClick={() => onAddToCart(product)}
      >
        {product.stock > 0 ? 'Add to Cart' : 'Out of Stock'}
      </button>
    </div>
  );
});

//Cart Rules & Validation
const updateQuantity = (productId, newQty, stock) => {
  if (newQty > stock) {
    alert("Cannot exceed available stock");
    return;
  }
  if (newQty < 1) return removeFromCart(productId);
  
  setCart(prev => prev.map(item => 
    item.id === productId ? { ...item, quantity: newQty } : item
  ));
};

//Suggested CSS (Basic Modules)
/* ProductList.module.css */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
}
