#react #js 
## RadioButton Component

```react
class RadioButton extends Component {
  render() {
    return (
      <label htmlFor={this.props.value}>
        <input id={this.props.value} type="radio" name={this.props.name} value={this.props.value}/>
        <span>{this.props.children}</span>
      </label>
    )
  }
}
```

## RadioGroup Component Wrap

```react
class RadioGroup extends React.Component {
  state={
    name:"G1"
  }
  renderChildren=()=>{
    return React.Children.map(this.props.children,child=>{
      return React.cloneElement(child,{
        name:this.state.name
      })
    })
  }
  render() {
    return (
      <div className="group">
        {React.Children.count(this.props.children)}
        {this.renderChildren()}
      </div>
    )
  }
}
```

```react
<RadioGroup>
    <RadioButton value="first">First</RadioButton>
    <RadioButton value="second">Second</RadioButton>
    <RadioButton value="third">Third</RadioButton>
</RadioGroup>
```

