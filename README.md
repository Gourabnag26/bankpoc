import * as React from 'react';
import { Route, Routes } from 'react-router-dom';
import { IRouter } from '../../model';

export interface RouterProps {
  pages: IRouter[];
}

export const Router = ({ pages }: RouterProps) => {
  const pageRoutes = pages.map(
    ({ path = '', title, element, childRoutes }: IRouter) => {
      return (
        <Route element={element} key={title} path={`${path}`}>
          {childRoutes &&
            childRoutes.map(
              ({ path, title, element, childRoutes }: IRouter) => (
                <Route element={element} key={title} path={`${path}`}>
                  {childRoutes &&
                    childRoutes.map(
                      ({ path, title, element, childRoutes }: IRouter) => (
                        <Route element={element} key={title} path={`${path}`} />
                      )
                    )}
                </Route>
              )
            )}
        </Route>
      );
    }
  );

  return <Routes>{pageRoutes}</Routes>;
};
