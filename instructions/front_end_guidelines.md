# Front-End Development Guidelines

## Table of Contents
1. [Component Architecture](#component-architecture)
2. [Styling Guidelines](#styling-guidelines)
3. [State Management](#state-management)
4. [Performance Optimization](#performance-optimization)
5. [Code Style](#code-style)
6. [Testing Guidelines](#testing-guidelines)

## Component Architecture

### Component Structure
```typescript
// components/features/PropertyCard.tsx
import React from 'react';
import { Property } from '@/types/property';
import { formatPrice } from '@/utils/formatters';

interface PropertyCardProps {
  property: Property;
  onSelect: (id: string) => void;
}

export const PropertyCard: React.FC<PropertyCardProps> = ({
  property,
  onSelect,
}) => {
  // Hooks at the top
  const [isHovered, setIsHovered] = React.useState(false);

  // Event handlers
  const handleClick = () => onSelect(property.id);

  // Render methods
  const renderPrice = () => (
    <div className="text-xl font-bold text-primary">
      {formatPrice(property.price)}
    </div>
  );

  return (
    <div
      className="property-card"
      onClick={handleClick}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <img src={property.imageUrl} alt={property.title} />
      <div className="property-info">
        <h3>{property.title}</h3>
        {renderPrice()}
      </div>
    </div>
  );
};
```

### Component Naming Conventions
- Use PascalCase for component names: `PropertyCard`, `SearchBar`
- Use camelCase for file names: `propertyCard.tsx`, `searchBar.tsx`
- Prefix custom hooks with 'use': `usePropertyData`, `useSearchFilters`

## Styling Guidelines

### CSS Modules
```typescript
// PropertyCard.module.css
.propertyCard {
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease;
}

.propertyCard:hover {
  transform: translateY(-4px);
}

.propertyInfo {
  padding: 1rem;
}

// PropertyCard.tsx
import styles from './PropertyCard.module.css';

<div className={styles.propertyCard}>
  <div className={styles.propertyInfo}>
    {/* content */}
  </div>
</div>
```

### Tailwind CSS Classes
```typescript
// Use consistent class ordering
<div className="
  // Layout
  flex flex-col gap-4
  
  // Spacing
  p-4 m-2
  
  // Typography
  text-lg font-semibold text-gray-800
  
  // Visual
  bg-white rounded-lg shadow-md
  
  // Interactive
  hover:shadow-lg transition-shadow
">
```

## State Management

### Local State
```typescript
// Use useState for simple state
const [isOpen, setIsOpen] = useState(false);

// Use useReducer for complex state
const [state, dispatch] = useReducer(reducer, initialState);
```

### Global State (Redux)
```typescript
// store/slices/propertySlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface PropertyState {
  properties: Property[];
  loading: boolean;
  error: string | null;
}

const initialState: PropertyState = {
  properties: [],
  loading: false,
  error: null,
};

const propertySlice = createSlice({
  name: 'property',
  initialState,
  reducers: {
    setProperties: (state, action: PayloadAction<Property[]>) => {
      state.properties = action.payload;
    },
    setLoading: (state, action: PayloadAction<boolean>) => {
      state.loading = action.payload;
    },
    setError: (state, action: PayloadAction<string | null>) => {
      state.error = action.payload;
    },
  },
});
```

## Performance Optimization

### Memoization
```typescript
// Memoize expensive computations
const memoizedValue = useMemo(() => 
  expensiveComputation(props.data), 
  [props.data]
);

// Memoize callbacks
const handleClick = useCallback(() => {
  // handle click
}, [/* dependencies */]);

// Memoize components
const MemoizedComponent = React.memo(Component);
```

### Code Splitting
```typescript
// Lazy load components
const PropertyDetails = React.lazy(() => 
  import('./PropertyDetails')
);

// Use Suspense
<Suspense fallback={<LoadingSpinner />}>
  <PropertyDetails />
</Suspense>
```

## Code Style

### TypeScript
```typescript
// Use interfaces for object types
interface Property {
  id: string;
  title: string;
  price: number;
  location: {
    city: string;
    state: string;
  };
}

// Use type for unions and intersections
type PropertyStatus = 'available' | 'sold' | 'pending';
type PropertyWithStatus = Property & { status: PropertyStatus };
```

### Error Handling
```typescript
// Use try-catch with async/await
try {
  const data = await fetchPropertyData(id);
  setProperty(data);
} catch (error) {
  setError(error instanceof Error ? error.message : 'An error occurred');
}

// Use error boundaries
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}
```

## Testing Guidelines

### Component Testing
```typescript
// PropertyCard.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { PropertyCard } from './PropertyCard';

describe('PropertyCard', () => {
  const mockProperty = {
    id: '1',
    title: 'Luxury Villa',
    price: 1000000,
  };

  it('renders property information correctly', () => {
    render(<PropertyCard property={mockProperty} onSelect={() => {}} />);
    
    expect(screen.getByText('Luxury Villa')).toBeInTheDocument();
    expect(screen.getByText('$1,000,000')).toBeInTheDocument();
  });

  it('calls onSelect when clicked', () => {
    const onSelect = jest.fn();
    render(<PropertyCard property={mockProperty} onSelect={onSelect} />);
    
    fireEvent.click(screen.getByRole('article'));
    expect(onSelect).toHaveBeenCalledWith('1');
  });
});
```

### Custom Hook Testing
```typescript
// usePropertyData.test.ts
import { renderHook, act } from '@testing-library/react-hooks';
import { usePropertyData } from './usePropertyData';

describe('usePropertyData', () => {
  it('fetches property data successfully', async () => {
    const { result } = renderHook(() => usePropertyData('1'));

    expect(result.current.loading).toBe(true);
    
    await act(async () => {
      await result.current.fetchData();
    });

    expect(result.current.loading).toBe(false);
    expect(result.current.data).toBeDefined();
  });
});
```

## Best Practices Summary

1. **Component Organization**
   - Keep components small and focused
   - Use composition over inheritance
   - Follow the single responsibility principle

2. **State Management**
   - Use local state for component-specific data
   - Use global state only for shared data
   - Implement proper state updates and side effects

3. **Performance**
   - Implement proper memoization
   - Use code splitting for large components
   - Optimize images and assets

4. **TypeScript**
   - Use strict type checking
   - Define proper interfaces and types
   - Avoid using 'any' type

5. **Testing**
   - Write unit tests for components
   - Test edge cases and error scenarios
   - Maintain good test coverage

6. **Code Style**
   - Follow consistent naming conventions
   - Write clean, self-documenting code
   - Use proper error handling

7. **Accessibility**
   - Use semantic HTML
   - Implement proper ARIA attributes
   - Ensure keyboard navigation works

Remember to:
- Keep code DRY (Don't Repeat Yourself)
- Write meaningful comments
- Follow the project's established patterns
- Review code before submitting
- Consider performance implications
- Maintain accessibility standards
