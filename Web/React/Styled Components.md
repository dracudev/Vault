```jsx
import styled from 'styled-components'

const Button = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid #BF4F74;
  color: #BF4F74;
  margin: 0.5em 1em;
  padding: 0.25em 1em;

  ${props => props.$primary && css`
    background: #BF4F74;
    color: white;
  `}
`;

const Container = styled.div`
  text-align: center;
`

export function Component($primary) {
	return (
	<>
		<Button $primary={$primary}></Button>
		<Container></Container>
	</>
	)
}

render(
  <Container>
    <Button>Normal Button</Button>
    <Button $primary>Primary Button</Button>
  </Container>
);
```