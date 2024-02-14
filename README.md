import React, { useState } from 'react';
import { ApolloClient, gql } from 'apollo-client';
import { List } from 'react-list';

// API URL'sini ve sorgularını değiştirin
const client = new ApolloClient({
  uri: 'https://studio.apollographql.com/public/countries/home?variant=current',
});

const App = () => {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  const fetchResults = () => {
    client.query({
      query: gql`
        query Countries {
          countries(search: "${query}") {
            name
            capital
            population
          }
        }
      `,
    }).then((response) => {
      setResults(response.data.countries);
    });
  };

  useEffect(() => {
    fetchResults();
  }, [query]);

  return (
    <div>
      <input type="text" value={query} onChange={(e) => setQuery(e.target.value)} />
      <List
        items={results}
        renderItem={(item) => (
          <div key={item.name}>{item.
