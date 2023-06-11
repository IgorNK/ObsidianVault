Passing object literal:
```
// Functional:
const Product = (props) => (
    <div>
        <p>{props.productData.name}</p> {/* Гидрокостюм для дайвинга */}
        <span>{props.productData.price}</span> {/* 14299 */}
    </div>
);

// Class:
class Product extends React.Component {
	render() {
		return (
			<div>
		        <p>{this.props.productData.name}</p> {/* Гидрокостюм для дайвинга */}
		        <span>{this.props.productData.price}</span> {/* 14299 */}
		    </div>
		);
	}
}

const ShoppingCart = (props) => (
    <>
        <h1>Корзина товаров</h1>
        <Product productData={{ name: "Гидрокостюм для дайвинга", price: 14299 }} />
    </>
); 
```

Passing primitives:
```
// Functional:
const Product = (props) => (
    <div>
        <p>{props.name}</p> {/* Гидрокостюм для дайвинга */}
        <span>{props.price}</span> {/* 14299 */}
    </div>
);

// Class:
class Product extends React.Component {
	render() {
		return (
			<div>
				<p>{this.props.name}</p> {/* Гидрокостюм для дайвинга */}
		        <span>{this.props.price}</span> {/* 14299 */}
		    </div>
		);
	}
}

// Использование компонента
<Product name="Гидрокостюм для дайвинга" price={14299} /> 
```

Rest parameters using spread syntax (...):
```
// These are identical. First two - pass down primitives, second two - rest parameters.
// Functional:
function CustomerPage(props) {
  return (
        <ProfileInfo 
            firstName={props.profileData.firstName} 
            lastName={props.profileData.lastName} 
        />
    );
}

// Class:
class CustomerPage extends React.Component {
	render() {
		return (
	        <ProfileInfo 
            firstName={this.props.profileData.firstName} 
            lastName={this.props.profileData.lastName} 
	        />
	    );
	}
}

// Functional:
function CustomerPage(props) {
  return <ProfileInfo {...props.profileData} />;
}

// Class:
class CustomerPage extends React.Component {
	render() {
		return <ProfileInfo {...this.props.profileData} />;
	}
}
```

Filtering parameters:
```
// This way size and userId properties are filtered out and not passed on to <input> HTML element.
const Input = props => {
  const { size, userId, ...otherProps } = props;  
    const className = size === "default" ? "DefaultInput" : "SmallInput";
  return <input className={className} {...otherProps} />;
};

const LandingPage = () => {
	return (
	    <>
            <b>Введите промокод:</b>     
            <Input size="default" type="text" disabled={false} userId={1112983} />    
	    </>
	);
}; 
```



#react