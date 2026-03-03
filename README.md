# Population--The-new-challenge



Set-Content app.py @"
import dash
from dash import dcc, html, Input, Output
import plotly.graph_objs as go
from datetime import datetime

app = dash.Dash(__name__)
server = app.server

# 2026 Population Constants
START_POP_2026 = 8200000000
NET_GAIN_PER_SECOND = 2.45 

app.layout = html.Div(style={'backgroundColor': '#0a0a0a', 'color': '#ffffff', 'padding': '40px', 'fontFamily': 'Segoe UI, sans-serif'}, children=[
    html.Div(style={'textAlign': 'center', 'marginBottom': '40px'}, children=[
        html.H1("🌍 Population: The New Challenge", style={'fontSize': '48px', 'fontWeight': 'bold'}),
        html.P("Real-time simulation and future growth forecasting.", style={'color': '#888', 'fontSize': '18px'}),
    ]),

    html.Div(style={'backgroundColor': '#161616', 'padding': '30px', 'borderRadius': '15px', 'textAlign': 'center', 'boxShadow': '0px 4px 20px rgba(0,255,204,0.1)'}, children=[
        html.H2("Live Estimated Count", style={'margin': '0', 'fontSize': '24px', 'color': '#00ffcc'}),
        html.H1(id='live-counter', style={'fontSize': '72px', 'margin': '10px 0', 'textShadow': '0 0 10px #00ffcc'}),
    ]),

    dcc.Graph(id='prediction-graph', config={'displayModeBar': False}, style={'marginTop': '20px'}),
    
    html.Footer(style={'textAlign': 'center', 'marginTop': '50px', 'borderTop': '1px solid #333', 'paddingTop': '20px'}, children=[
        html.P("Crafted with love in Mumbai", style={'fontStyle': 'italic', 'color': '#555'}),
        html.A("View Source on GitHub", href="https://github.com/adityadhage1/Population--The-new-challenge", 
               target="_blank", style={'color': '#00ffcc', 'textDecoration': 'none', 'fontWeight': 'bold'})
    ]),

    dcc.Interval(id='interval', interval=1000)
])

@app.callback(
    [Output('live-counter', 'children'), Output('prediction-graph', 'figure')],
    [Input('interval', 'n_intervals')]
)
def update_dashboard(n):
    current_pop = START_POP_2026 + (n * NET_GAIN_PER_SECOND)
    
    # Simple growth projection data
    years = [2026, 2030, 2040, 2050, 2060]
    pop_values = [8.2e9, 8.5e9, 9.1e9, 9.7e9, 10.2e9]
    
    fig = go.Figure(go.Scatter(x=years, y=pop_values, mode='lines+markers', 
                               line=dict(color='#00ffcc', width=4),
                               marker=dict(size=10, color='#ffffff')))
    
    fig.update_layout(
        template="plotly_dark",
        title="Global Population Incline (2026-2060)",
        paper_bgcolor='rgba(0,0,0,0)',
        plot_bgcolor='rgba(0,0,0,0)',
        xaxis=dict(gridcolor='#333'),
        yaxis=dict(gridcolor='#333', title="Total Population")
    )
    
    return f"{int(current_pop):,}", fig

if __name__ == '__main__':
    app.run(debug=True)
"@
