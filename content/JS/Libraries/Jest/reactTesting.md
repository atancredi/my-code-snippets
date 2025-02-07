# Testing React components with Jest

## 1. Utilize the Arrange-Act-Assert (AAA) pattern:

The AAA pattern is a fundamental principle in testing that helps maintain clarity and organization in your test cases. Arrange sets the test, Act performs the action or interaction, and Assert verifies the expected outcome. This pattern enhances readability and makes it easier to identify issues when they occur.

# 2. Test components in isolation:

Testing individual components ensures thorough examination without interference from other parts, making debugging more accessible and faster.

Example: In the Assert section, a test for a child component in the same test of the parent component would be considered a bad practice.

# 3. Use snapshot testing cautiously:

Snapshot testing allows you to capture the rendered output of your components and compare it against a previously saved snapshot. This helps you detect unintended changes in your UI. Jest has built-in support for snapshot testing.

# 4. Mock external dependencies:

If your component relies on external dependencies, such as API calls or modules, it’s best to mock them during testing to isolate the component’s behaviour. Tools like Jest provide mocking capabilities to help you easily mock dependencies.

Mocking an API call with Jest and Axios
```JavaScript
import axios from ‘axios’;

jest.mock(‘axios’);
test(‘Component fetches data from API correctly’, async () => {
	axios.get.mockResolvedValue({ data: ‘mocked data’ });
	const { getByText } = render(<DataFetchingComponent />);
	await waitFor(() => {
		expect(getByText(‘Data: mocked data’)).toBeInTheDocument();
	});
	
});

```






---
# sources
https://medium.com/@elightwalk/testing-react-components-best-practices-and-tools-cc1ca1021181