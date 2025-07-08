# Pattern: React Context (Shared State)

## VIBES Rating
`<🔍🧶💧>`

## Description
React Context provides a way to share values between components without prop drilling, but creates hidden dependencies and runtime coupling between components.

## Example
```javascript
// Context creates invisible wiring between components
const ThemeContext = React.createContext();
const UserContext = React.createContext();
const FeatureFlagContext = React.createContext();

function App() {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState(null);
  const [flags, setFlags] = useState({});
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <UserContext.Provider value={{ user, setUser }}>
        <FeatureFlagContext.Provider value={flags}>
          <Dashboard />
        </FeatureFlagContext.Provider>
      </UserContext.Provider>
    </ThemeContext.Provider>
  );
}

function Dashboard() {
  // Multiple context dependencies
  const { theme } = useContext(ThemeContext);
  const { user } = useContext(UserContext);
  const flags = useContext(FeatureFlagContext);
  
  // Component behavior depends on multiple contexts
  if (!user) return <Login />;
  if (!flags.dashboardEnabled) return <ComingSoon />;
  
  return <div className={theme}>...</div>;
}

function SettingsPanel() {
  const { theme, setTheme } = useContext(ThemeContext);
  const { user, setUser } = useContext(UserContext);
  
  // Mutations affect other components invisibly
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
    // This change affects Dashboard and any other consumers
  };
}
```

## Why This Rating

### Expressive Power: 🔍
- Clear conceptual model: Context provides shared state
- Well-documented patterns for usage
- Multiple approaches (useContext, Consumer, HOCs)
- React guides toward proper usage

### Context Flow: 🧶
- Hidden dependencies through context
- Components coupled to context structure
- Changes propagate invisibly
- Can't understand component in isolation

### Error Surface: 💧
- Missing context providers cause runtime errors
- Type safety depends on TypeScript discipline
- Performance issues from unnecessary re-renders
- Errors caught during component render

## Common Occurrence
- React applications avoiding prop drilling
- Theme management systems
- User authentication state
- Feature flag systems
- Global app configuration

## Impact
- LLMs struggle with invisible dependencies
- Testing requires extensive mocking
- Refactoring affects unknown components
- Performance debugging is complex

## Related Patterns
- [redux-store.md](./redux-store.md) - More explicit alternative
- [prop-drilling.md](./prop-drilling.md) - What this pattern avoids
- [module-pattern.md](./module-pattern.md) - Better isolation approach

## Consensus Data
- Model Agreement: 89%
- Disputed Aspects: Some rate 🌀 when contexts reference each other
- Test Date: July 2025
