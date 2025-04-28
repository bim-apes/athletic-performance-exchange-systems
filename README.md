# athletic-performance-exchange-systems
BIM - Athletic Performance Exchange (BIM-APEX)
SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET transaction_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET xmloption = content;
SET client_min_messages = warning;
SET row_security = off;
SET default_tablespace = '';
SET default_table_access_method = heap;

CREATE TABLE public.access_tokens (
    id integer NOT NULL,
    signature_id integer NOT NULL,
    token text NOT NULL,
    expires_at timestamp without time zone NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'active'::character varying
);

ALTER TABLE public.access_tokens OWNER TO neondb_owner;

CREATE SEQUENCE public.access_tokens_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.access_tokens_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.access_tokens_id_seq OWNED BY public.access_tokens.id;

CREATE TABLE public.ad_impressions (
    id integer NOT NULL,
    ad_id integer NOT NULL,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    user_id integer,
    ip_address character varying(45),
    user_agent text
);

ALTER TABLE public.ad_impressions OWNER TO neondb_owner;

ALTER TABLE public.ad_impressions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.ad_impressions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.ad_zones (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    placement character varying(50) NOT NULL,
    description text,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.ad_zones OWNER TO neondb_owner;

ALTER TABLE public.ad_zones ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.ad_zones_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.ads (
    id integer NOT NULL,
    zone_id integer NOT NULL,
    title character varying(200) NOT NULL,
    content text NOT NULL,
    advertiser_name character varying(100) NOT NULL,
    start_date date NOT NULL,
    end_date date,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.ads OWNER TO neondb_owner;

ALTER TABLE public.ads ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.ads_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.advanced_metric_correlations (
    id integer NOT NULL,
    primary_metric_id integer,
    secondary_metric_id integer,
    correlation_type character varying(50),
    correlation_value numeric(5,2),
    time_lag interval,
    statistical_significance numeric(5,2),
    sample_size integer,
    analysis_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    seasonality_adjusted boolean DEFAULT false,
    trend_adjusted boolean DEFAULT false,
    cross_correlation_data jsonb
);

ALTER TABLE public.advanced_metric_correlations OWNER TO neondb_owner;

ALTER TABLE public.advanced_metric_correlations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.advanced_metric_correlations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.algo_backtests (
    id integer NOT NULL,
    user_id integer NOT NULL,
    configuration_id integer,
    start_date timestamp without time zone NOT NULL,
    end_date timestamp without time zone NOT NULL,
    performance_metrics jsonb DEFAULT '{}'::jsonb,
    trades_summary jsonb DEFAULT '[]'::jsonb,
    risk_metrics jsonb DEFAULT '{}'::jsonb,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.algo_backtests OWNER TO neondb_owner;

ALTER TABLE public.algo_backtests ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.algo_backtests_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.algo_configurations (
    id integer NOT NULL,
    user_id integer NOT NULL,
    strategy_name character varying(100) NOT NULL,
    indicators jsonb DEFAULT '[]'::jsonb,
    timeframes jsonb DEFAULT '[]'::jsonb,
    risk_parameters jsonb DEFAULT '{}'::jsonb,
    backtest_results jsonb DEFAULT '{}'::jsonb,
    is_active boolean DEFAULT false,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    performance_weights jsonb DEFAULT '{"speed": 0.2, "stamina": 0.2, "accuracy": 0.2, "consistency": 0.2, "improvement": 0.2}'::jsonb,
    performance_thresholds jsonb DEFAULT '{"speed_min": 0, "stamina_min": 0.6, "accuracy_min": 0.7, "consistency_min": 0.8, "improvement_min": 0.1}'::jsonb,
    signal_generation_rules jsonb DEFAULT '{"exit": {"stop_loss": true, "threshold": 0.3}, "entry": {"threshold": 0.7, "confirmation_needed": true}}'::jsonb
);

ALTER TABLE public.algo_configurations OWNER TO neondb_owner;

ALTER TABLE public.algo_configurations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.algo_configurations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.api_analysis (
    id integer NOT NULL,
    user_id integer NOT NULL,
    endpoint text NOT NULL,
    response_data jsonb NOT NULL,
    analysis_results text,
    analysis_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    completed_at timestamp without time zone
);

ALTER TABLE public.api_analysis OWNER TO neondb_owner;

ALTER TABLE public.api_analysis ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.api_analysis_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.auth_accounts (
    id integer NOT NULL,
    "userId" integer NOT NULL,
    type character varying(255) NOT NULL,
    provider character varying(255) NOT NULL,
    "providerAccountId" character varying(255) NOT NULL,
    refresh_token text,
    access_token text,
    expires_at bigint,
    id_token text,
    scope text,
    session_state text,
    token_type text,
    password text
);

ALTER TABLE public.auth_accounts OWNER TO neondb_owner;

CREATE SEQUENCE public.auth_accounts_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.auth_accounts_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.auth_accounts_id_seq OWNED BY public.auth_accounts.id;

CREATE TABLE public.auth_sessions (
    id integer NOT NULL,
    "userId" integer NOT NULL,
    expires timestamp with time zone NOT NULL,
    "sessionToken" character varying(255) NOT NULL
);

ALTER TABLE public.auth_sessions OWNER TO neondb_owner;

CREATE SEQUENCE public.auth_sessions_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.auth_sessions_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.auth_sessions_id_seq OWNED BY public.auth_sessions.id;

CREATE TABLE public.auth_users (
    id integer NOT NULL,
    name character varying(255),
    email character varying(255),
    "emailVerified" timestamp with time zone,
    image text
);

ALTER TABLE public.auth_users OWNER TO neondb_owner;

CREATE SEQUENCE public.auth_users_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.auth_users_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.auth_users_id_seq OWNED BY public.auth_users.id;

CREATE TABLE public.auth_verification_token (
    identifier text NOT NULL,
    expires timestamp with time zone NOT NULL,
    token text NOT NULL
);

ALTER TABLE public.auth_verification_token OWNER TO neondb_owner;

CREATE TABLE public.blockchain_assets (
    id integer NOT NULL,
    network_id integer,
    contract_id integer,
    asset_id character varying(255) NOT NULL,
    asset_type character varying(100) NOT NULL,
    owner_id integer,
    metadata jsonb NOT NULL,
    status character varying(50) NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.blockchain_assets OWNER TO neondb_owner;

ALTER TABLE public.blockchain_assets ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.blockchain_assets_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.blockchain_networks (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    network_type character varying(50) NOT NULL,
    connection_profile jsonb NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.blockchain_networks OWNER TO neondb_owner;

ALTER TABLE public.blockchain_networks ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.blockchain_networks_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.blockchain_transactions (
    id integer NOT NULL,
    network_id integer,
    contract_id integer,
    transaction_id character varying(255) NOT NULL,
    transaction_type character varying(50) NOT NULL,
    function_name character varying(100) NOT NULL,
    parameters jsonb,
    status character varying(50) NOT NULL,
    result jsonb,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    user_id integer
);

ALTER TABLE public.blockchain_transactions OWNER TO neondb_owner;

ALTER TABLE public.blockchain_transactions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.blockchain_transactions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.budget_goals (
    id integer NOT NULL,
    user_id integer,
    category character varying(50) NOT NULL,
    target_amount numeric(10,2) NOT NULL,
    current_amount numeric(10,2) DEFAULT 0,
    start_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    end_date timestamp without time zone,
    status character varying(20) DEFAULT 'active'::character varying,
    priority integer DEFAULT 1
);

ALTER TABLE public.budget_goals OWNER TO neondb_owner;

ALTER TABLE public.budget_goals ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.budget_goals_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.club_announcements (
    id integer NOT NULL,
    club_id integer NOT NULL,
    title character varying(200) NOT NULL,
    content text NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    priority character varying(20) DEFAULT 'normal'::character varying
);

ALTER TABLE public.club_announcements OWNER TO neondb_owner;

ALTER TABLE public.club_announcements ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.club_announcements_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.club_events (
    id integer NOT NULL,
    club_id integer NOT NULL,
    title character varying(200) NOT NULL,
    description text,
    event_date timestamp without time zone NOT NULL,
    location text,
    max_participants integer,
    registration_deadline timestamp without time zone,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'upcoming'::character varying
);

ALTER TABLE public.club_events OWNER TO neondb_owner;

ALTER TABLE public.club_events ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.club_events_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.club_memberships (
    id integer NOT NULL,
    club_id integer,
    tier_id integer,
    start_date timestamp without time zone NOT NULL,
    end_date timestamp without time zone,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.club_memberships OWNER TO neondb_owner;

ALTER TABLE public.club_memberships ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.club_memberships_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.club_roles (
    id integer NOT NULL,
    club_id integer NOT NULL,
    user_id integer NOT NULL,
    role_type character varying(50) NOT NULL,
    assigned_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.club_roles OWNER TO neondb_owner;

ALTER TABLE public.club_roles ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.club_roles_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.clubs (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    location character varying(100),
    founded_year integer,
    logo_url text,
    website_url text,
    description text,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    genre character varying(50),
    is_college_club boolean DEFAULT false,
    college_name character varying(200),
    college_department character varying(200),
    advisor_name character varying(100),
    advisor_email character varying(100),
    meeting_schedule text,
    social_media_links jsonb,
    membership_requirements text,
    club_type character varying(50) DEFAULT 'general'::character varying
);

ALTER TABLE public.clubs OWNER TO neondb_owner;

ALTER TABLE public.clubs ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.clubs_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.company_metrics (
    id integer NOT NULL,
    metric_name character varying(100) NOT NULL,
    metric_value character varying(100) NOT NULL,
    display_order integer,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.company_metrics OWNER TO neondb_owner;

ALTER TABLE public.company_metrics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.company_metrics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.connected_devices (
    id integer NOT NULL,
    user_id integer NOT NULL,
    device_id character varying(50) NOT NULL,
    device_name character varying(100),
    device_type character varying(50),
    battery_level integer,
    last_connected timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'connected'::character varying
);

ALTER TABLE public.connected_devices OWNER TO neondb_owner;

CREATE SEQUENCE public.connected_devices_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.connected_devices_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.connected_devices_id_seq OWNED BY public.connected_devices.id;

CREATE TABLE public.consultancy_plans (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    description text,
    price_yearly numeric(10,2) NOT NULL,
    features jsonb DEFAULT '[]'::jsonb,
    stripe_product_id text,
    stripe_price_id text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    target_audience character varying(100),
    service_type character varying(50),
    is_popular boolean DEFAULT false,
    additional_features jsonb
);

ALTER TABLE public.consultancy_plans OWNER TO neondb_owner;

ALTER TABLE public.consultancy_plans ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.consultancy_plans_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.consultancy_service_features (
    id integer NOT NULL,
    plan_id integer,
    feature_name character varying(200) NOT NULL,
    feature_description text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.consultancy_service_features OWNER TO neondb_owner;

ALTER TABLE public.consultancy_service_features ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.consultancy_service_features_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.consultancy_subscriptions (
    id integer NOT NULL,
    user_id integer NOT NULL,
    plan_id integer NOT NULL,
    stripe_subscription_id text NOT NULL,
    status character varying(20) DEFAULT 'active'::character varying NOT NULL,
    current_period_start timestamp without time zone,
    current_period_end timestamp without time zone,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    payment_status character varying(50) DEFAULT 'active'::character varying,
    last_payment_failure_date timestamp without time zone,
    payment_failure_count integer DEFAULT 0,
    usage_limit_minutes integer,
    usage_current_minutes integer DEFAULT 0
);

ALTER TABLE public.consultancy_subscriptions OWNER TO neondb_owner;

ALTER TABLE public.consultancy_subscriptions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.consultancy_subscriptions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.contact_submissions (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    email character varying(100) NOT NULL,
    subject character varying(200) NOT NULL,
    message text NOT NULL,
    status character varying(20) DEFAULT 'pending'::character varying NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.contact_submissions OWNER TO neondb_owner;

ALTER TABLE public.contact_submissions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.contact_submissions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.content_waivers (
    id integer NOT NULL,
    user_id integer NOT NULL,
    content_id text NOT NULL,
    full_name character varying(100) NOT NULL,
    email character varying(100) NOT NULL,
    company character varying(100) NOT NULL,
    purpose text NOT NULL,
    signature_data text NOT NULL,
    agreement_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'active'::character varying
);

ALTER TABLE public.content_waivers OWNER TO neondb_owner;

ALTER TABLE public.content_waivers ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.content_waivers_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.customer_subscriptions (
    id integer NOT NULL,
    user_id integer,
    plan_id integer,
    stripe_subscription_id text NOT NULL,
    stripe_customer_id text NOT NULL,
    status character varying(20) DEFAULT 'active'::character varying NOT NULL,
    current_period_start timestamp without time zone,
    current_period_end timestamp without time zone,
    cancel_at_period_end boolean DEFAULT false,
    currency character varying(3) DEFAULT 'USD'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.customer_subscriptions OWNER TO neondb_owner;

CREATE SEQUENCE public.customer_subscriptions_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.customer_subscriptions_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.customer_subscriptions_id_seq OWNED BY public.customer_subscriptions.id;

CREATE TABLE public.digital_signatures (
    id integer NOT NULL,
    user_id integer NOT NULL,
    file_url text NOT NULL,
    file_hash text NOT NULL,
    signature text NOT NULL,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    blockchain_hash text,
    status character varying(20) DEFAULT 'active'::character varying
);

ALTER TABLE public.digital_signatures OWNER TO neondb_owner;

CREATE SEQUENCE public.digital_signatures_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.digital_signatures_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.digital_signatures_id_seq OWNED BY public.digital_signatures.id;

CREATE TABLE public.email_notifications (
    id integer NOT NULL,
    user_id integer,
    notification_type character varying(50) NOT NULL,
    subject text NOT NULL,
    content text NOT NULL,
    status character varying(20) DEFAULT 'pending'::character varying,
    sent_at timestamp without time zone,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    metadata jsonb DEFAULT '{}'::jsonb
);

ALTER TABLE public.email_notifications OWNER TO neondb_owner;

ALTER TABLE public.email_notifications ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.email_notifications_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.email_preferences (
    id integer NOT NULL,
    user_id integer,
    welcome_emails boolean DEFAULT true,
    trade_confirmations boolean DEFAULT true,
    performance_alerts boolean DEFAULT true,
    account_updates boolean DEFAULT true,
    marketing_emails boolean DEFAULT true,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.email_preferences OWNER TO neondb_owner;

ALTER TABLE public.email_preferences ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.email_preferences_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.enhanced_performance_metrics (
    id integer NOT NULL,
    player_id integer,
    metric_type character varying(50),
    raw_value numeric(10,2),
    normalized_value numeric(5,2),
    z_score numeric(5,2),
    momentum_indicator numeric(5,2),
    volatility_index numeric(5,2),
    seasonality_factor numeric(5,2),
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    sport_type character varying(50),
    competition_level character varying(50),
    environmental_conditions jsonb,
    fatigue_index numeric(5,2),
    recovery_score numeric(5,2),
    consistency_rating numeric(5,2)
);

ALTER TABLE public.enhanced_performance_metrics OWNER TO neondb_owner;

ALTER TABLE public.enhanced_performance_metrics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.enhanced_performance_metrics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.event_registrations (
    id integer NOT NULL,
    event_id integer,
    user_id integer,
    registration_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'confirmed'::character varying
);

ALTER TABLE public.event_registrations OWNER TO neondb_owner;

ALTER TABLE public.event_registrations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.event_registrations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.financial_scaling (
    id integer NOT NULL,
    team_id integer,
    total_investment numeric(15,2),
    liquidity_reserve numeric(15,2),
    risk_adjusted_value numeric(15,2),
    last_valuation timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    scaling_status character varying(20) DEFAULT 'active'::character varying,
    ai_valuation_metrics jsonb DEFAULT '{}'::jsonb,
    real_time_metrics jsonb DEFAULT '{}'::jsonb,
    institutional_tier character varying(50) DEFAULT 'standard'::character varying
);

ALTER TABLE public.financial_scaling OWNER TO neondb_owner;

ALTER TABLE public.financial_scaling ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.financial_scaling_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.investment_indices (
    id integer NOT NULL,
    tournament_id integer,
    team_id integer,
    performance_score numeric(5,2),
    liquidity_score numeric(5,2),
    risk_rating numeric(5,2),
    investment_index numeric(5,2),
    calculated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    institutional_rating numeric(5,2),
    ai_confidence_score numeric(5,2),
    real_time_performance jsonb DEFAULT '{}'::jsonb
);

ALTER TABLE public.investment_indices OWNER TO neondb_owner;

ALTER TABLE public.investment_indices ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.investment_indices_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.investment_portfolios (
    id integer NOT NULL,
    user_id integer NOT NULL,
    name character varying(100) NOT NULL,
    risk_level character varying(20) NOT NULL,
    target_allocation jsonb,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    ai_risk_analysis jsonb,
    ai_recommendations jsonb,
    last_ai_analysis_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    tier_level character varying(20) DEFAULT 'bronze'::character varying,
    rebalance_frequency interval DEFAULT '24:00:00'::interval,
    last_rebalanced timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.investment_portfolios OWNER TO neondb_owner;

ALTER TABLE public.investment_portfolios ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.investment_portfolios_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.investment_transactions (
    id integer NOT NULL,
    investment_id integer NOT NULL,
    transaction_type character varying(20) NOT NULL,
    quantity numeric(20,8) NOT NULL,
    price numeric(20,2) NOT NULL,
    total_amount numeric(20,2) NOT NULL,
    transaction_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'completed'::character varying
);

ALTER TABLE public.investment_transactions OWNER TO neondb_owner;

ALTER TABLE public.investment_transactions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.investment_transactions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.investments (
    id integer NOT NULL,
    portfolio_id integer NOT NULL,
    asset_type character varying(50) NOT NULL,
    asset_name character varying(100) NOT NULL,
    quantity numeric(20,8) NOT NULL,
    purchase_price numeric(20,2) NOT NULL,
    current_price numeric(20,2),
    purchase_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'active'::character varying,
    last_updated timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT check_positive_price CHECK ((purchase_price > (0)::numeric)),
    CONSTRAINT check_positive_quantity CHECK ((quantity >= (0)::numeric)),
    CONSTRAINT check_valid_current_price CHECK (((current_price IS NULL) OR (current_price > (0)::numeric)))
);

ALTER TABLE public.investments OWNER TO neondb_owner;

ALTER TABLE public.investments ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.investments_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.investor_inquiries (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    email character varying(100) NOT NULL,
    company character varying(100),
    investment_amount numeric(15,2),
    message text NOT NULL,
    status character varying(20) DEFAULT 'pending'::character varying NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.investor_inquiries OWNER TO neondb_owner;

ALTER TABLE public.investor_inquiries ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.investor_inquiries_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.learn_progress (
    id integer NOT NULL,
    user_id integer,
    module_name character varying(100) NOT NULL,
    completion_percentage integer DEFAULT 0,
    credits_earned integer DEFAULT 0,
    last_updated timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.learn_progress OWNER TO neondb_owner;

CREATE SEQUENCE public.learn_progress_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.learn_progress_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.learn_progress_id_seq OWNED BY public.learn_progress.id;

CREATE TABLE public.learning_content (
    id integer NOT NULL,
    title character varying(200) NOT NULL,
    content_type character varying(50) NOT NULL,
    description text,
    content text NOT NULL,
    difficulty_level character varying(20),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    section_order integer,
    parent_section integer,
    is_section boolean DEFAULT false,
    handbook_section character varying(100)
);

ALTER TABLE public.learning_content OWNER TO neondb_owner;

ALTER TABLE public.learning_content ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.learning_content_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.license_validations (
    id integer NOT NULL,
    user_id integer,
    wallet_address text NOT NULL,
    license_status boolean DEFAULT false,
    issued_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    expires_at timestamp without time zone,
    last_validated timestamp without time zone,
    validation_hash text,
    tier_level character varying(50) DEFAULT 'standard'::character varying,
    blockchain_address text,
    smart_contract_address text,
    blockchain_transaction_hash text
);

ALTER TABLE public.license_validations OWNER TO neondb_owner;

ALTER TABLE public.license_validations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.license_validations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.market_analysis (
    id integer NOT NULL,
    performance_metric_id integer,
    market_signal character varying(50),
    signal_strength numeric(5,2),
    prediction_confidence numeric(5,2),
    analysis_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    factors jsonb,
    alert_level character varying(20),
    signal_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    is_active boolean DEFAULT true
);

ALTER TABLE public.market_analysis OWNER TO neondb_owner;

ALTER TABLE public.market_analysis ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.market_analysis_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.market_data (
    id integer NOT NULL,
    ticker character varying(10) NOT NULL,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    open numeric(10,2),
    high numeric(10,2),
    low numeric(10,2),
    close numeric(10,2),
    volume bigint
);

ALTER TABLE public.market_data OWNER TO neondb_owner;

ALTER TABLE public.market_data ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.market_data_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.market_sponsorships (
    id integer NOT NULL,
    region character varying(100) NOT NULL,
    sponsor_address text NOT NULL,
    investment_amount numeric(15,2) NOT NULL,
    currency_code character varying(3) DEFAULT 'USD'::character varying,
    status character varying(20) DEFAULT 'active'::character varying,
    smart_contract_address text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    last_validated timestamp without time zone,
    risk_score numeric(5,2),
    market_conditions jsonb,
    real_time_valuation numeric(15,2),
    institutional_metrics jsonb DEFAULT '{}'::jsonb,
    performance_index numeric(5,2)
);

ALTER TABLE public.market_sponsorships OWNER TO neondb_owner;

ALTER TABLE public.market_sponsorships ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.market_sponsorships_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.membership_tiers (
    id integer NOT NULL,
    name character varying(50) NOT NULL,
    description text,
    price numeric(10,2) NOT NULL,
    billing_cycle character varying(20) NOT NULL,
    features jsonb NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    benefits jsonb DEFAULT '[]'::jsonb,
    access_level integer DEFAULT 1,
    annual_price numeric(10,2),
    billing_interval character varying(20) DEFAULT 'monthly'::character varying
);

ALTER TABLE public.membership_tiers OWNER TO neondb_owner;

ALTER TABLE public.membership_tiers ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.membership_tiers_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.metric_achievements (
    id integer NOT NULL,
    time_scale_metric_id integer NOT NULL,
    player_id integer NOT NULL,
    achieved_value numeric(10,2) NOT NULL,
    achieved_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    points_earned numeric(10,2) NOT NULL,
    telemetry_data jsonb
);

ALTER TABLE public.metric_achievements OWNER TO neondb_owner;

ALTER TABLE public.metric_achievements ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.metric_achievements_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.metric_market_correlations (
    id integer NOT NULL,
    metric_type character varying(50),
    market_signal character varying(50),
    correlation_coefficient numeric(5,2),
    confidence_level numeric(5,2),
    analysis_period character varying(20),
    last_updated timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.metric_market_correlations OWNER TO neondb_owner;

ALTER TABLE public.metric_market_correlations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.metric_market_correlations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.mobile_app_settings (
    id integer NOT NULL,
    user_id integer NOT NULL,
    notification_preferences jsonb DEFAULT '{"trades": true, "performance": true, "achievements": true}'::jsonb,
    theme_preference character varying(20) DEFAULT 'system'::character varying,
    data_sync_frequency integer DEFAULT 15,
    offline_mode_enabled boolean DEFAULT false,
    last_sync_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    device_token text
);

ALTER TABLE public.mobile_app_settings OWNER TO neondb_owner;

ALTER TABLE public.mobile_app_settings ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.mobile_app_settings_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.mobile_metric_cache (
    id integer NOT NULL,
    user_id integer NOT NULL,
    metric_type character varying(50) NOT NULL,
    cache_key text NOT NULL,
    cache_data jsonb NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    expires_at timestamp without time zone NOT NULL
);

ALTER TABLE public.mobile_metric_cache OWNER TO neondb_owner;

ALTER TABLE public.mobile_metric_cache ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.mobile_metric_cache_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.mobile_sessions (
    id integer NOT NULL,
    user_id integer NOT NULL,
    device_id character varying(50) NOT NULL,
    session_start timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    session_end timestamp without time zone,
    battery_start integer,
    battery_end integer,
    network_type character varying(20),
    app_version character varying(20)
);

ALTER TABLE public.mobile_sessions OWNER TO neondb_owner;

ALTER TABLE public.mobile_sessions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.mobile_sessions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.onboarding_rewards (
    id integer NOT NULL,
    user_id integer NOT NULL,
    reward_type character varying(50) NOT NULL,
    reward_amount integer NOT NULL,
    step_earned character varying(50) NOT NULL,
    claimed_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    expires_at timestamp without time zone,
    status character varying(20) DEFAULT 'active'::character varying
);

ALTER TABLE public.onboarding_rewards OWNER TO neondb_owner;

ALTER TABLE public.onboarding_rewards ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.onboarding_rewards_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.onboarding_settings (
    id integer NOT NULL,
    user_id integer NOT NULL,
    investment_style character varying(50),
    risk_tolerance character varying(50),
    preferred_sports jsonb DEFAULT '[]'::jsonb,
    investment_goals jsonb DEFAULT '[]'::jsonb,
    completed_steps jsonb DEFAULT '[]'::jsonb,
    rewards_claimed jsonb DEFAULT '{}'::jsonb,
    last_step_completed timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    trading_frequency character varying(50),
    risk_metrics jsonb DEFAULT '{"maxDrawdown": 0.1, "positionSize": 0.02, "leverageLimit": 1}'::jsonb,
    algo_preferences jsonb DEFAULT '{"indicators": [], "strategies": [], "timeframes": [], "automationLevel": "semi"}'::jsonb
);

ALTER TABLE public.onboarding_settings OWNER TO neondb_owner;

ALTER TABLE public.onboarding_settings ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.onboarding_settings_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.payment_history (
    id integer NOT NULL,
    subscription_id integer,
    stripe_payment_intent_id text,
    stripe_invoice_id text,
    amount numeric(10,2) NOT NULL,
    currency character varying(3) DEFAULT 'USD'::character varying,
    status character varying(20) NOT NULL,
    payment_method_type character varying(50),
    invoice_url text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.payment_history OWNER TO neondb_owner;

CREATE SEQUENCE public.payment_history_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.payment_history_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.payment_history_id_seq OWNED BY public.payment_history.id;

CREATE TABLE public.payment_recovery_attempts (
    id integer NOT NULL,
    subscription_id integer,
    failure_reason character varying(255),
    attempt_count integer DEFAULT 0,
    last_attempt_date timestamp without time zone,
    next_attempt_date timestamp without time zone,
    status character varying(50) DEFAULT 'pending'::character varying,
    recovery_strategy character varying(50),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.payment_recovery_attempts OWNER TO neondb_owner;

ALTER TABLE public.payment_recovery_attempts ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.payment_recovery_attempts_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_alert_configs (
    id integer NOT NULL,
    user_id integer,
    metric_type character varying(50),
    alert_condition jsonb,
    threshold_value numeric(10,2),
    alert_priority character varying(20),
    notification_channels jsonb,
    cooldown_period interval,
    last_triggered timestamp without time zone,
    is_active boolean DEFAULT true,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    custom_message_template text,
    alert_category character varying(50),
    compound_conditions jsonb
);

ALTER TABLE public.performance_alert_configs OWNER TO neondb_owner;

ALTER TABLE public.performance_alert_configs ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_alert_configs_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_alert_history (
    id integer NOT NULL,
    alert_config_id integer,
    triggered_value numeric(10,2),
    alert_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    notification_status character varying(20),
    response_time interval,
    alert_context jsonb,
    resolution_status character varying(20),
    resolution_timestamp timestamp without time zone,
    impact_score numeric(5,2)
);

ALTER TABLE public.performance_alert_history OWNER TO neondb_owner;

ALTER TABLE public.performance_alert_history ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_alert_history_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_metrics (
    id integer NOT NULL,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    sensor_id character varying(50) NOT NULL,
    speed numeric(10,2),
    force numeric(10,2),
    distance numeric(10,2),
    passes integer,
    scores integer,
    efficiency_score numeric(10,2),
    reaction_time numeric(10,2),
    stamina numeric(10,2),
    tactical_rating numeric(10,2),
    tackles integer,
    match_type character varying(50),
    opponent character varying(100),
    match_date date,
    notification_status character varying(20) DEFAULT 'unnotified'::character varying,
    alert_threshold numeric(10,2),
    last_notification_time timestamp without time zone,
    shooting_accuracy numeric(5,2),
    flag_pull_success numeric(5,2),
    reaction_time_ms integer,
    player_id integer,
    session_date date DEFAULT CURRENT_DATE,
    sport_type character varying(50),
    spin_rate numeric(10,2),
    velocity numeric(10,2),
    force_impact numeric(10,2),
    trajectory_data jsonb
);

ALTER TABLE public.performance_metrics OWNER TO neondb_owner;

CREATE TABLE public.performance_metrics_history (
    id integer NOT NULL,
    metric_id integer,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    metric_type character varying(50),
    old_value numeric(10,2),
    new_value numeric(10,2),
    change_percentage numeric(5,2)
);

ALTER TABLE public.performance_metrics_history OWNER TO neondb_owner;

ALTER TABLE public.performance_metrics_history ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_metrics_history_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE SEQUENCE public.performance_metrics_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.performance_metrics_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.performance_metrics_id_seq OWNED BY public.performance_metrics.id;

CREATE TABLE public.performance_ml_predictions (
    id integer NOT NULL,
    metric_id integer,
    model_type character varying(50),
    prediction_value numeric(10,2),
    confidence_score numeric(5,2),
    feature_importance jsonb,
    prediction_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    validation_score numeric(5,2),
    error_margin numeric(5,2),
    model_version character varying(20),
    training_window_start timestamp without time zone,
    training_window_end timestamp without time zone
);

ALTER TABLE public.performance_ml_predictions OWNER TO neondb_owner;

ALTER TABLE public.performance_ml_predictions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_ml_predictions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_predictions (
    id integer NOT NULL,
    tournament_session_id integer,
    predicted_performance numeric(5,2),
    confidence_score numeric(5,2),
    prediction_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    actual_performance numeric(5,2),
    prediction_accuracy numeric(5,2)
);

ALTER TABLE public.performance_predictions OWNER TO neondb_owner;

ALTER TABLE public.performance_predictions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_predictions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_spending_analytics (
    id integer NOT NULL,
    user_id integer,
    analysis_period character varying(20),
    total_yield numeric(10,2),
    total_spending numeric(10,2),
    roi_percentage numeric(5,2),
    top_performing_categories text[],
    improvement_suggestions text[],
    analysis_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.performance_spending_analytics OWNER TO neondb_owner;

ALTER TABLE public.performance_spending_analytics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_spending_analytics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_trends (
    id integer NOT NULL,
    metric_type character varying(50),
    trend_direction character varying(20),
    trend_strength numeric(5,2),
    start_date timestamp without time zone,
    end_date timestamp without time zone,
    analysis_notes jsonb,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.performance_trends OWNER TO neondb_owner;

ALTER TABLE public.performance_trends ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_trends_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.performance_yields (
    id integer NOT NULL,
    user_id integer,
    performance_metric_id integer,
    yield_amount numeric(10,2) NOT NULL,
    yield_type character varying(50) NOT NULL,
    earned_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    status character varying(20) DEFAULT 'pending'::character varying,
    payout_date timestamp without time zone,
    notes text,
    CONSTRAINT check_positive_yield CHECK ((yield_amount > (0)::numeric)),
    CONSTRAINT check_valid_yield_status CHECK (((status)::text = ANY ((ARRAY['pending'::character varying, 'paid'::character varying, 'cancelled'::character varying])::text[])))
);

ALTER TABLE public.performance_yields OWNER TO neondb_owner;

ALTER TABLE public.performance_yields ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.performance_yields_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.pitch_deck_slides (
    id integer NOT NULL,
    title character varying(200) NOT NULL,
    content text NOT NULL,
    display_order integer,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.pitch_deck_slides OWNER TO neondb_owner;

ALTER TABLE public.pitch_deck_slides ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.pitch_deck_slides_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.player_devices (
    id integer NOT NULL,
    player_id integer,
    device_id character varying(50) NOT NULL,
    device_type character varying(50) NOT NULL,
    connection_status character varying(20) DEFAULT 'disconnected'::character varying,
    last_connected timestamp without time zone,
    battery_level integer,
    firmware_version character varying(20),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.player_devices OWNER TO neondb_owner;

ALTER TABLE public.player_devices ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.player_devices_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.player_heatmaps (
    id integer NOT NULL,
    performance_metric_id integer,
    x_coordinate numeric(10,2),
    y_coordinate numeric(10,2),
    intensity numeric(5,2),
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.player_heatmaps OWNER TO neondb_owner;

ALTER TABLE public.player_heatmaps ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.player_heatmaps_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.players (
    id integer NOT NULL,
    user_id integer,
    team_id integer,
    club_id integer,
    jersey_number integer,
    "position" character varying(50),
    height numeric(5,2),
    weight numeric(5,2),
    dominant_foot character varying(10),
    birth_date date,
    nationality character varying(100),
    bio text,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.players OWNER TO neondb_owner;

ALTER TABLE public.players ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.players_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.playtime_correlations (
    id integer NOT NULL,
    player_id integer,
    metric_type character varying(50),
    playtime_minutes integer,
    performance_impact numeric(5,2),
    correlation_strength numeric(5,2),
    optimal_range_min integer,
    optimal_range_max integer,
    analysis_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.playtime_correlations OWNER TO neondb_owner;

ALTER TABLE public.playtime_correlations ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.playtime_correlations_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.playtime_metrics (
    id integer NOT NULL,
    player_id integer,
    session_date date DEFAULT CURRENT_DATE,
    duration_minutes integer NOT NULL,
    intensity_level character varying(20),
    sport_type character varying(50),
    performance_score numeric(5,2),
    fatigue_level numeric(5,2),
    recovery_needed integer,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.playtime_metrics OWNER TO neondb_owner;

ALTER TABLE public.playtime_metrics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.playtime_metrics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.playtime_thresholds (
    id integer NOT NULL,
    metric_type character varying(50) NOT NULL,
    min_threshold numeric(5,2),
    optimal_threshold numeric(5,2),
    max_threshold numeric(5,2),
    rest_period_minutes integer,
    sport_type character varying(50),
    intensity_level character varying(20),
    age_group character varying(20),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.playtime_thresholds OWNER TO neondb_owner;

ALTER TABLE public.playtime_thresholds ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.playtime_thresholds_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.position_history (
    id integer NOT NULL,
    position_id integer,
    user_id integer,
    ticker character varying(10) NOT NULL,
    position_type character varying(10) NOT NULL,
    quantity integer NOT NULL,
    entry_price numeric(10,2) NOT NULL,
    exit_price numeric(10,2) NOT NULL,
    pnl numeric(10,2) NOT NULL,
    entry_date timestamp without time zone NOT NULL,
    exit_date timestamp without time zone NOT NULL,
    close_reason character varying(20) NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.position_history OWNER TO neondb_owner;

ALTER TABLE public.position_history ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.position_history_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.pricing_plans (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    description text,
    price_yearly numeric(10,2) NOT NULL,
    features jsonb DEFAULT '[]'::jsonb,
    is_popular boolean DEFAULT false,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.pricing_plans OWNER TO neondb_owner;

ALTER TABLE public.pricing_plans ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.pricing_plans_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.promotional_offers (
    id integer NOT NULL,
    offer_type character varying(50) NOT NULL,
    title character varying(200) NOT NULL,
    description text,
    discount_amount numeric(10,2),
    code character varying(50),
    start_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    end_date timestamp without time zone,
    is_active boolean DEFAULT true,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.promotional_offers OWNER TO neondb_owner;

ALTER TABLE public.promotional_offers ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.promotional_offers_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.request_tracking (
    id integer NOT NULL,
    ip_address character varying(45) NOT NULL,
    endpoint character varying(255) NOT NULL,
    request_count integer DEFAULT 1,
    first_request_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    last_request_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.request_tracking OWNER TO neondb_owner;

ALTER TABLE public.request_tracking ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.request_tracking_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.revenue_transactions (
    id integer NOT NULL,
    club_id integer,
    transaction_type character varying(50) NOT NULL,
    amount numeric(10,2) NOT NULL,
    currency character varying(3) DEFAULT 'USD'::character varying,
    status character varying(20) DEFAULT 'pending'::character varying,
    description text,
    payment_method character varying(50),
    payment_id text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.revenue_transactions OWNER TO neondb_owner;

ALTER TABLE public.revenue_transactions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.revenue_transactions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.security_events (
    id integer NOT NULL,
    user_id integer,
    event_type character varying(50) NOT NULL,
    severity character varying(20) NOT NULL,
    description text NOT NULL,
    metadata jsonb DEFAULT '{}'::jsonb,
    ip_address character varying(45),
    user_agent text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    analyzed boolean DEFAULT false,
    ai_analysis jsonb DEFAULT '{}'::jsonb
);

ALTER TABLE public.security_events OWNER TO neondb_owner;

ALTER TABLE public.security_events ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.security_events_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.simulation_logs (
    id integer NOT NULL,
    user_id integer,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    signal character varying(10) NOT NULL,
    action character varying(10) NOT NULL,
    predicted_roi numeric(10,2),
    metrics jsonb
);

ALTER TABLE public.simulation_logs OWNER TO neondb_owner;

CREATE SEQUENCE public.simulation_logs_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.simulation_logs_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.simulation_logs_id_seq OWNED BY public.simulation_logs.id;

CREATE TABLE public.sleep_mode_analytics (
    id integer NOT NULL,
    user_id integer,
    session_id text NOT NULL,
    device_type character varying(50),
    browser_info text,
    event_type character varying(50) NOT NULL,
    event_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    duration_seconds integer,
    interaction_type character varying(50),
    custom_settings jsonb,
    battery_level integer,
    page_url text,
    variant_id integer
);

ALTER TABLE public.sleep_mode_analytics OWNER TO neondb_owner;

ALTER TABLE public.sleep_mode_analytics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.sleep_mode_analytics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.sleep_mode_test_variants (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    description text,
    settings jsonb NOT NULL,
    start_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    end_date timestamp without time zone,
    is_active boolean DEFAULT true,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    advanced_settings jsonb DEFAULT '{"ui": {"theme": "default", "fontScale": 1.0, "animations": true, "colorScheme": "auto"}, "behavior": {"smartWakeup": false, "autoActivation": false, "gradualDimming": true, "notificationHandling": "mute"}, "performance": {"cacheStrategy": "aggressive", "preloadAssets": true, "renderOptimization": "balanced"}, "accessibility": {"highContrast": false, "screenReader": true, "reducedMotion": false, "keyboardNavigation": true}}'::jsonb,
    metrics_config jsonb DEFAULT '{"goals": {"engagementTarget": 300, "satisfactionThreshold": 4.5}, "customEvents": [], "primaryMetrics": ["engagementTime", "userSatisfaction"], "secondaryMetrics": ["batteryImpact", "performanceScore"]}'::jsonb,
    targeting_rules jsonb DEFAULT '{"deviceTypes": ["all"], "userSegments": ["all"], "timeConstraints": null, "geographicRegions": ["all"]}'::jsonb
);

ALTER TABLE public.sleep_mode_test_variants OWNER TO neondb_owner;

ALTER TABLE public.sleep_mode_test_variants ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.sleep_mode_test_variants_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.sleep_mode_user_variants (
    id integer NOT NULL,
    user_id integer,
    variant_id integer,
    assigned_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.sleep_mode_user_variants OWNER TO neondb_owner;

ALTER TABLE public.sleep_mode_user_variants ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.sleep_mode_user_variants_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.smart_ball_devices (
    id integer NOT NULL,
    device_id character varying(50) NOT NULL,
    ball_type character varying(50) NOT NULL,
    user_id integer,
    last_calibration timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    firmware_version character varying(20),
    battery_level integer,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    stream_enabled boolean DEFAULT false,
    stream_config jsonb DEFAULT '{}'::jsonb,
    last_stream_id character varying(100),
    streaming_since timestamp without time zone
);

ALTER TABLE public.smart_ball_devices OWNER TO neondb_owner;

ALTER TABLE public.smart_ball_devices ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.smart_ball_devices_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.smart_ball_events (
    id integer NOT NULL,
    device_id integer,
    event_type character varying(50) NOT NULL,
    event_data jsonb NOT NULL,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    tournament_id integer,
    mining_job_id integer,
    mining_status character varying(20) DEFAULT 'pending'::character varying,
    mining_start_time timestamp without time zone,
    mining_end_time timestamp without time zone,
    mining_rewards numeric(10,2)
);

ALTER TABLE public.smart_ball_events OWNER TO neondb_owner;

ALTER TABLE public.smart_ball_events ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.smart_ball_events_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.smart_contracts (
    id integer NOT NULL,
    network_id integer,
    name character varying(100) NOT NULL,
    version character varying(50) NOT NULL,
    channel_name character varying(100) NOT NULL,
    contract_type character varying(50) NOT NULL,
    endorsement_policy jsonb,
    source_code text NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.smart_contracts OWNER TO neondb_owner;

ALTER TABLE public.smart_contracts ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.smart_contracts_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.spending_tracker (
    id integer NOT NULL,
    user_id integer,
    category character varying(50) NOT NULL,
    amount numeric(10,2) NOT NULL,
    description text,
    spent_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    receipt_url text,
    is_recurring boolean DEFAULT false,
    recurring_interval character varying(20),
    tags text[]
);

ALTER TABLE public.spending_tracker OWNER TO neondb_owner;

ALTER TABLE public.spending_tracker ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.spending_tracker_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.sponsorship_tokens (
    id integer NOT NULL,
    athlete_id integer,
    token_amount numeric(20,8),
    token_type character varying(50),
    allocation_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    smart_contract_address text,
    blockchain_transaction_hash text,
    status character varying(20) DEFAULT 'active'::character varying,
    valuation_at_mint numeric(15,2)
);

ALTER TABLE public.sponsorship_tokens OWNER TO neondb_owner;

ALTER TABLE public.sponsorship_tokens ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.sponsorship_tokens_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.store_order_items (
    id integer NOT NULL,
    order_id integer NOT NULL,
    product_id integer NOT NULL,
    quantity integer NOT NULL,
    price_at_time numeric(10,2) NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.store_order_items OWNER TO neondb_owner;

CREATE SEQUENCE public.store_order_items_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.store_order_items_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.store_order_items_id_seq OWNED BY public.store_order_items.id;

CREATE TABLE public.store_orders (
    id integer NOT NULL,
    user_id integer NOT NULL,
    shipping_address text NOT NULL,
    shipping_city character varying(100) NOT NULL,
    shipping_state character varying(50) NOT NULL,
    shipping_zip character varying(20) NOT NULL,
    status character varying(20) DEFAULT 'pending'::character varying NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.store_orders OWNER TO neondb_owner;

CREATE SEQUENCE public.store_orders_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.store_orders_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.store_orders_id_seq OWNED BY public.store_orders.id;

CREATE TABLE public.store_products (
    id integer NOT NULL,
    name character varying(255) NOT NULL,
    price numeric(10,2) NOT NULL,
    description text,
    image_url text,
    category character varying(50) NOT NULL,
    stock_quantity integer DEFAULT 0,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.store_products OWNER TO neondb_owner;

CREATE SEQUENCE public.store_products_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.store_products_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.store_products_id_seq OWNED BY public.store_products.id;

CREATE TABLE public.subscription_analytics (
    id integer NOT NULL,
    subscription_id integer,
    period_start timestamp without time zone NOT NULL,
    period_end timestamp without time zone NOT NULL,
    total_usage_minutes integer DEFAULT 0,
    feature_usage jsonb DEFAULT '{}'::jsonb,
    revenue_amount numeric(10,2),
    churn_risk_score numeric(3,2),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.subscription_analytics OWNER TO neondb_owner;

ALTER TABLE public.subscription_analytics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.subscription_analytics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.subscription_features (
    id integer NOT NULL,
    subscription_id integer,
    feature_name character varying(100) NOT NULL,
    usage_limit integer,
    current_usage integer DEFAULT 0,
    reset_period character varying(20) DEFAULT 'monthly'::character varying,
    last_reset timestamp with time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.subscription_features OWNER TO neondb_owner;

CREATE SEQUENCE public.subscription_features_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.subscription_features_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.subscription_features_id_seq OWNED BY public.subscription_features.id;

CREATE TABLE public.subscription_limits (
    id integer NOT NULL,
    plan_type character varying(50) NOT NULL,
    feature_name character varying(100) NOT NULL,
    monthly_limit integer,
    created_at timestamp with time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.subscription_limits OWNER TO neondb_owner;

CREATE SEQUENCE public.subscription_limits_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.subscription_limits_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.subscription_limits_id_seq OWNED BY public.subscription_limits.id;

CREATE TABLE public.subscription_payments (
    id integer NOT NULL,
    user_id integer,
    subscription_id integer,
    stripe_payment_id text NOT NULL,
    amount numeric(10,2) NOT NULL,
    status character varying(20) NOT NULL,
    payment_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    payment_method character varying(50),
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.subscription_payments OWNER TO neondb_owner;

ALTER TABLE public.subscription_payments ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.subscription_payments_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.subscription_plans (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    description text,
    price_monthly numeric(10,2) NOT NULL,
    price_yearly numeric(10,2) NOT NULL,
    currency character varying(3) DEFAULT 'USD'::character varying,
    stripe_product_id text,
    stripe_price_id_monthly text,
    stripe_price_id_yearly text,
    features jsonb DEFAULT '[]'::jsonb,
    is_active boolean DEFAULT true,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.subscription_plans OWNER TO neondb_owner;

CREATE SEQUENCE public.subscription_plans_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.subscription_plans_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.subscription_plans_id_seq OWNED BY public.subscription_plans.id;

CREATE TABLE public.subscription_usage (
    id integer NOT NULL,
    subscription_id integer,
    feature_name character varying(100) NOT NULL,
    usage_count integer DEFAULT 1,
    usage_timestamp timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    metadata jsonb DEFAULT '{}'::jsonb
);

ALTER TABLE public.subscription_usage OWNER TO neondb_owner;

ALTER TABLE public.subscription_usage ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.subscription_usage_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.subscriptions (
    id integer NOT NULL,
    user_id integer,
    stripe_subscription_id text NOT NULL,
    plan_type character varying(50) NOT NULL,
    status character varying(20) DEFAULT 'active'::character varying NOT NULL,
    start_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    end_date timestamp without time zone,
    billing_cycle character varying(20) NOT NULL,
    amount numeric(10,2) NOT NULL,
    last_payment_date timestamp without time zone,
    next_payment_date timestamp without time zone,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    trial_ends_at timestamp with time zone,
    trial_started_at timestamp with time zone
);

ALTER TABLE public.subscriptions OWNER TO neondb_owner;

ALTER TABLE public.subscriptions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.subscriptions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.suspicious_patterns (
    id integer NOT NULL,
    pattern_type character varying(50) NOT NULL,
    pattern_data jsonb NOT NULL,
    severity character varying(20) NOT NULL,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    active boolean DEFAULT true
);

ALTER TABLE public.suspicious_patterns OWNER TO neondb_owner;

ALTER TABLE public.suspicious_patterns ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.suspicious_patterns_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.team_members (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    role character varying(100) NOT NULL,
    bio text,
    image_url text,
    linkedin_url text,
    display_order integer,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.team_members OWNER TO neondb_owner;

ALTER TABLE public.team_members ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.team_members_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.teams (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    location character varying(100),
    founded_year integer,
    logo_url text,
    website_url text,
    description text,
    status character varying(20) DEFAULT 'active'::character varying,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.teams OWNER TO neondb_owner;

ALTER TABLE public.teams ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.teams_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.telemetry_data (
    id integer NOT NULL,
    device_id character varying(50) NOT NULL,
    user_id integer,
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    speed numeric(10,2),
    force numeric(10,2),
    spin_rate numeric(10,2),
    velocity numeric(10,2),
    trajectory_data jsonb,
    session_id character varying(50),
    sport_type character varying(50),
    device_type character varying(50),
    battery_level integer,
    data_quality character varying(20) DEFAULT 'good'::character varying
);

ALTER TABLE public.telemetry_data OWNER TO neondb_owner;

ALTER TABLE public.telemetry_data ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.telemetry_data_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.telemetry_metrics (
    id integer NOT NULL,
    stream_id character varying(100) NOT NULL,
    metric_type character varying(50) NOT NULL,
    metric_value numeric(10,2),
    collected_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    aggregation_period character varying(20) DEFAULT '1min'::character varying,
    sample_count integer DEFAULT 1
);

ALTER TABLE public.telemetry_metrics OWNER TO neondb_owner;

CREATE SEQUENCE public.telemetry_metrics_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.telemetry_metrics_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.telemetry_metrics_id_seq OWNED BY public.telemetry_metrics.id;

CREATE TABLE public.telemetry_stream (
    id integer NOT NULL,
    device_id character varying(50) NOT NULL,
    user_id integer,
    stream_id character varying(100) NOT NULL,
    stream_status character varying(20) DEFAULT 'active'::character varying,
    started_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    ended_at timestamp without time zone,
    stream_config jsonb DEFAULT '{}'::jsonb,
    error_count integer DEFAULT 0,
    last_error text,
    last_telemetry_at timestamp without time zone,
    metrics_collected integer DEFAULT 0
);

ALTER TABLE public.telemetry_stream OWNER TO neondb_owner;

CREATE SEQUENCE public.telemetry_stream_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.telemetry_stream_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.telemetry_stream_id_seq OWNED BY public.telemetry_stream.id;

CREATE TABLE public.time_scale_metrics (
    id integer NOT NULL,
    metric_name character varying(100) NOT NULL,
    time_scale character varying(20) NOT NULL,
    threshold_value numeric(10,2),
    measurement_unit character varying(50),
    time_window interval NOT NULL,
    points_value numeric(10,2),
    description text,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT time_scale_metrics_time_scale_check CHECK (((time_scale)::text = ANY ((ARRAY['short_term'::character varying, 'mid_term'::character varying, 'long_term'::character varying])::text[])))
);

ALTER TABLE public.time_scale_metrics OWNER TO neondb_owner;

ALTER TABLE public.time_scale_metrics ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.time_scale_metrics_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.token_rewards (
    id integer NOT NULL,
    metric_id integer,
    token_type character varying(50),
    reward_amount numeric(10,2),
    "timestamp" timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    metric_source character varying(50),
    performance_score numeric(5,2),
    smart_ball_id character varying(50),
    achievement_milestone character varying(100)
);

ALTER TABLE public.token_rewards OWNER TO neondb_owner;

CREATE SEQUENCE public.token_rewards_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.token_rewards_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.token_rewards_id_seq OWNED BY public.token_rewards.id;

CREATE TABLE public.tournament_mining_jobs (
    id integer NOT NULL,
    tournament_name character varying(100) NOT NULL,
    start_date timestamp without time zone NOT NULL,
    end_date timestamp without time zone,
    status character varying(20) DEFAULT 'active'::character varying,
    reward_pool numeric(10,2) DEFAULT 0,
    min_participants integer DEFAULT 1,
    required_metrics jsonb,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.tournament_mining_jobs OWNER TO neondb_owner;

ALTER TABLE public.tournament_mining_jobs ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.tournament_mining_jobs_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.tournament_participants (
    id integer NOT NULL,
    tournament_id integer,
    device_id integer,
    user_id integer,
    join_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    total_mining_time interval DEFAULT '00:00:00'::interval,
    total_rewards numeric(10,2) DEFAULT 0,
    status character varying(20) DEFAULT 'active'::character varying,
    last_active timestamp without time zone
);

ALTER TABLE public.tournament_participants OWNER TO neondb_owner;

ALTER TABLE public.tournament_participants ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.tournament_participants_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.tournament_sessions (
    id integer NOT NULL,
    tournament_id integer,
    user_id integer,
    device_id integer,
    start_time timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    end_time timestamp without time zone,
    status character varying(20) DEFAULT 'active'::character varying,
    performance_data jsonb DEFAULT '{}'::jsonb,
    token_rewards numeric(10,2) DEFAULT 0,
    mining_rate numeric(10,2) DEFAULT 0,
    last_update timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.tournament_sessions OWNER TO neondb_owner;

ALTER TABLE public.tournament_sessions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.tournament_sessions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.trading_positions (
    id integer NOT NULL,
    user_id integer NOT NULL,
    ticker character varying(10) NOT NULL,
    position_type character varying(10) NOT NULL,
    quantity integer NOT NULL,
    entry_price numeric(10,2) NOT NULL,
    entry_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    exit_price numeric(10,2),
    exit_date timestamp without time zone,
    status character varying(20) DEFAULT 'open'::character varying,
    pnl numeric(10,2),
    stop_loss numeric(10,2),
    take_profit numeric(10,2),
    CONSTRAINT check_positive_entry_price CHECK ((entry_price > (0)::numeric)),
    CONSTRAINT check_positive_trading_quantity CHECK ((quantity > 0)),
    CONSTRAINT check_valid_exit_price CHECK (((exit_price IS NULL) OR (exit_price > (0)::numeric))),
    CONSTRAINT check_valid_status CHECK (((status)::text = ANY ((ARRAY['open'::character varying, 'closed'::character varying])::text[])))
);

ALTER TABLE public.trading_positions OWNER TO neondb_owner;

ALTER TABLE public.trading_positions ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.trading_positions_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.user_achievements (
    id integer NOT NULL,
    user_id integer,
    achievement_type character varying(50),
    achievement_name character varying(100),
    description text,
    earned_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    points integer DEFAULT 0
);

ALTER TABLE public.user_achievements OWNER TO neondb_owner;

ALTER TABLE public.user_achievements ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.user_achievements_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.user_engagement (
    id integer NOT NULL,
    user_id integer NOT NULL,
    activity_type character varying(50) NOT NULL,
    activity_data jsonb,
    points_earned integer DEFAULT 0,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.user_engagement OWNER TO neondb_owner;

ALTER TABLE public.user_engagement ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.user_engagement_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.user_learning_progress (
    id integer NOT NULL,
    user_id integer NOT NULL,
    content_id integer NOT NULL,
    status character varying(20) DEFAULT 'not_started'::character varying,
    score integer,
    last_accessed timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    completion_date timestamp without time zone
);

ALTER TABLE public.user_learning_progress OWNER TO neondb_owner;

ALTER TABLE public.user_learning_progress ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.user_learning_progress_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.user_onboarding (
    id integer NOT NULL,
    user_id integer NOT NULL,
    tutorial_step character varying(50) NOT NULL,
    completed_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    feedback text
);

ALTER TABLE public.user_onboarding OWNER TO neondb_owner;

ALTER TABLE public.user_onboarding ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.user_onboarding_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

CREATE TABLE public.users (
    id integer NOT NULL,
    name character varying(100) NOT NULL,
    email character varying(100) NOT NULL,
    credits integer DEFAULT 0,
    created_at timestamp without time zone DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE public.users OWNER TO neondb_owner;

CREATE SEQUENCE public.users_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER SEQUENCE public.users_id_seq OWNER TO neondb_owner;

ALTER SEQUENCE public.users_id_seq OWNED BY public.users.id;

CREATE TABLE public.valuation_history (
    id integer NOT NULL,
    team_id integer,
    valuation_date timestamp without time zone DEFAULT CURRENT_TIMESTAMP,
    performance_metrics jsonb,
    financial_metrics jsonb,
    ai_predictions jsonb,
    confidence_score numeric(5,2)
);

ALTER TABLE public.valuation_history OWNER TO neondb_owner;

ALTER TABLE public.valuation_history ALTER COLUMN id ADD GENERATED ALWAYS AS IDENTITY (
    SEQUENCE NAME public.valuation_history_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1
);

ALTER TABLE ONLY public.access_tokens ALTER COLUMN id SET DEFAULT nextval('public.access_tokens_id_seq'::regclass);

ALTER TABLE ONLY public.auth_accounts ALTER COLUMN id SET DEFAULT nextval('public.auth_accounts_id_seq'::regclass);

ALTER TABLE ONLY public.auth_sessions ALTER COLUMN id SET DEFAULT nextval('public.auth_sessions_id_seq'::regclass);

ALTER TABLE ONLY public.auth_users ALTER COLUMN id SET DEFAULT nextval('public.auth_users_id_seq'::regclass);

ALTER TABLE ONLY public.connected_devices ALTER COLUMN id SET DEFAULT nextval('public.connected_devices_id_seq'::regclass);

ALTER TABLE ONLY public.customer_subscriptions ALTER COLUMN id SET DEFAULT nextval('public.customer_subscriptions_id_seq'::regclass);

ALTER TABLE ONLY public.digital_signatures ALTER COLUMN id SET DEFAULT nextval('public.digital_signatures_id_seq'::regclass);

ALTER TABLE ONLY public.learn_progress ALTER COLUMN id SET DEFAULT nextval('public.learn_progress_id_seq'::regclass);

ALTER TABLE ONLY public.payment_history ALTER COLUMN id SET DEFAULT nextval('public.payment_history_id_seq'::regclass);

ALTER TABLE ONLY public.performance_metrics ALTER COLUMN id SET DEFAULT nextval('public.performance_metrics_id_seq'::regclass);

ALTER TABLE ONLY public.simulation_logs ALTER COLUMN id SET DEFAULT nextval('public.simulation_logs_id_seq'::regclass);

ALTER TABLE ONLY public.store_order_items ALTER COLUMN id SET DEFAULT nextval('public.store_order_items_id_seq'::regclass);

ALTER TABLE ONLY public.store_orders ALTER COLUMN id SET DEFAULT nextval('public.store_orders_id_seq'::regclass);

ALTER TABLE ONLY public.store_products ALTER COLUMN id SET DEFAULT nextval('public.store_products_id_seq'::regclass);

ALTER TABLE ONLY public.subscription_features ALTER COLUMN id SET DEFAULT nextval('public.subscription_features_id_seq'::regclass);

ALTER TABLE ONLY public.subscription_limits ALTER COLUMN id SET DEFAULT nextval('public.subscription_limits_id_seq'::regclass);

ALTER TABLE ONLY public.subscription_plans ALTER COLUMN id SET DEFAULT nextval('public.subscription_plans_id_seq'::regclass);

ALTER TABLE ONLY public.telemetry_metrics ALTER COLUMN id SET DEFAULT nextval('public.telemetry_metrics_id_seq'::regclass);

ALTER TABLE ONLY public.telemetry_stream ALTER COLUMN id SET DEFAULT nextval('public.telemetry_stream_id_seq'::regclass);

ALTER TABLE ONLY public.token_rewards ALTER COLUMN id SET DEFAULT nextval('public.token_rewards_id_seq'::regclass);

ALTER TABLE ONLY public.users ALTER COLUMN id SET DEFAULT nextval('public.users_id_seq'::regclass);

ALTER TABLE ONLY public.access_tokens
    ADD CONSTRAINT access_tokens_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.ad_impressions
    ADD CONSTRAINT ad_impressions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.ad_zones
    ADD CONSTRAINT ad_zones_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.ads
    ADD CONSTRAINT ads_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.advanced_metric_correlations
    ADD CONSTRAINT advanced_metric_correlations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.algo_backtests
    ADD CONSTRAINT algo_backtests_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.algo_configurations
    ADD CONSTRAINT algo_configurations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.api_analysis
    ADD CONSTRAINT api_analysis_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.auth_accounts
    ADD CONSTRAINT auth_accounts_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.auth_sessions
    ADD CONSTRAINT auth_sessions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.auth_users
    ADD CONSTRAINT auth_users_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.auth_verification_token
    ADD CONSTRAINT auth_verification_token_pkey PRIMARY KEY (identifier, token);

ALTER TABLE ONLY public.blockchain_assets
    ADD CONSTRAINT blockchain_assets_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.blockchain_networks
    ADD CONSTRAINT blockchain_networks_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.blockchain_transactions
    ADD CONSTRAINT blockchain_transactions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.budget_goals
    ADD CONSTRAINT budget_goals_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.club_announcements
    ADD CONSTRAINT club_announcements_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.club_events
    ADD CONSTRAINT club_events_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.club_memberships
    ADD CONSTRAINT club_memberships_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.club_roles
    ADD CONSTRAINT club_roles_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.clubs
    ADD CONSTRAINT clubs_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.company_metrics
    ADD CONSTRAINT company_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.connected_devices
    ADD CONSTRAINT connected_devices_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.connected_devices
    ADD CONSTRAINT connected_devices_user_id_device_id_key UNIQUE (user_id, device_id);

ALTER TABLE ONLY public.consultancy_plans
    ADD CONSTRAINT consultancy_plans_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.consultancy_service_features
    ADD CONSTRAINT consultancy_service_features_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.consultancy_subscriptions
    ADD CONSTRAINT consultancy_subscriptions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.contact_submissions
    ADD CONSTRAINT contact_submissions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.content_waivers
    ADD CONSTRAINT content_waivers_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.customer_subscriptions
    ADD CONSTRAINT customer_subscriptions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.digital_signatures
    ADD CONSTRAINT digital_signatures_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.email_notifications
    ADD CONSTRAINT email_notifications_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.email_preferences
    ADD CONSTRAINT email_preferences_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.email_preferences
    ADD CONSTRAINT email_preferences_user_id_key UNIQUE (user_id);

ALTER TABLE ONLY public.enhanced_performance_metrics
    ADD CONSTRAINT enhanced_performance_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.event_registrations
    ADD CONSTRAINT event_registrations_event_id_user_id_key UNIQUE (event_id, user_id);

ALTER TABLE ONLY public.event_registrations
    ADD CONSTRAINT event_registrations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.financial_scaling
    ADD CONSTRAINT financial_scaling_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.investment_indices
    ADD CONSTRAINT investment_indices_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.investment_portfolios
    ADD CONSTRAINT investment_portfolios_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.investment_transactions
    ADD CONSTRAINT investment_transactions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.investments
    ADD CONSTRAINT investments_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.investor_inquiries
    ADD CONSTRAINT investor_inquiries_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.learn_progress
    ADD CONSTRAINT learn_progress_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.learning_content
    ADD CONSTRAINT learning_content_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.license_validations
    ADD CONSTRAINT license_validations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.market_analysis
    ADD CONSTRAINT market_analysis_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.market_data
    ADD CONSTRAINT market_data_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.market_sponsorships
    ADD CONSTRAINT market_sponsorships_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.membership_tiers
    ADD CONSTRAINT membership_tiers_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.metric_achievements
    ADD CONSTRAINT metric_achievements_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.metric_market_correlations
    ADD CONSTRAINT metric_market_correlations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.mobile_app_settings
    ADD CONSTRAINT mobile_app_settings_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.mobile_metric_cache
    ADD CONSTRAINT mobile_metric_cache_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.mobile_sessions
    ADD CONSTRAINT mobile_sessions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.onboarding_rewards
    ADD CONSTRAINT onboarding_rewards_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.onboarding_rewards
    ADD CONSTRAINT onboarding_rewards_user_id_step_earned_key UNIQUE (user_id, step_earned);

ALTER TABLE ONLY public.onboarding_settings
    ADD CONSTRAINT onboarding_settings_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.onboarding_settings
    ADD CONSTRAINT onboarding_settings_user_id_key UNIQUE (user_id);

ALTER TABLE ONLY public.payment_history
    ADD CONSTRAINT payment_history_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.payment_recovery_attempts
    ADD CONSTRAINT payment_recovery_attempts_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_alert_configs
    ADD CONSTRAINT performance_alert_configs_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_alert_history
    ADD CONSTRAINT performance_alert_history_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_metrics_history
    ADD CONSTRAINT performance_metrics_history_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_metrics
    ADD CONSTRAINT performance_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_ml_predictions
    ADD CONSTRAINT performance_ml_predictions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_predictions
    ADD CONSTRAINT performance_predictions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_spending_analytics
    ADD CONSTRAINT performance_spending_analytics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_trends
    ADD CONSTRAINT performance_trends_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.performance_yields
    ADD CONSTRAINT performance_yields_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.pitch_deck_slides
    ADD CONSTRAINT pitch_deck_slides_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.player_devices
    ADD CONSTRAINT player_devices_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.player_heatmaps
    ADD CONSTRAINT player_heatmaps_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.players
    ADD CONSTRAINT players_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.playtime_correlations
    ADD CONSTRAINT playtime_correlations_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.playtime_metrics
    ADD CONSTRAINT playtime_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.playtime_thresholds
    ADD CONSTRAINT playtime_thresholds_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.position_history
    ADD CONSTRAINT position_history_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.pricing_plans
    ADD CONSTRAINT pricing_plans_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.promotional_offers
    ADD CONSTRAINT promotional_offers_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.request_tracking
    ADD CONSTRAINT request_tracking_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.revenue_transactions
    ADD CONSTRAINT revenue_transactions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.security_events
    ADD CONSTRAINT security_events_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.simulation_logs
    ADD CONSTRAINT simulation_logs_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.sleep_mode_analytics
    ADD CONSTRAINT sleep_mode_analytics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.sleep_mode_test_variants
    ADD CONSTRAINT sleep_mode_test_variants_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.sleep_mode_user_variants
    ADD CONSTRAINT sleep_mode_user_variants_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.sleep_mode_user_variants
    ADD CONSTRAINT sleep_mode_user_variants_user_id_variant_id_key UNIQUE (user_id, variant_id);

ALTER TABLE ONLY public.smart_ball_devices
    ADD CONSTRAINT smart_ball_devices_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.smart_ball_events
    ADD CONSTRAINT smart_ball_events_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.smart_contracts
    ADD CONSTRAINT smart_contracts_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.spending_tracker
    ADD CONSTRAINT spending_tracker_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.sponsorship_tokens
    ADD CONSTRAINT sponsorship_tokens_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.store_order_items
    ADD CONSTRAINT store_order_items_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.store_orders
    ADD CONSTRAINT store_orders_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.store_products
    ADD CONSTRAINT store_products_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_analytics
    ADD CONSTRAINT subscription_analytics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_features
    ADD CONSTRAINT subscription_features_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_limits
    ADD CONSTRAINT subscription_limits_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_payments
    ADD CONSTRAINT subscription_payments_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_plans
    ADD CONSTRAINT subscription_plans_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscription_usage
    ADD CONSTRAINT subscription_usage_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.subscriptions
    ADD CONSTRAINT subscriptions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.suspicious_patterns
    ADD CONSTRAINT suspicious_patterns_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.team_members
    ADD CONSTRAINT team_members_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.teams
    ADD CONSTRAINT teams_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.telemetry_data
    ADD CONSTRAINT telemetry_data_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.telemetry_metrics
    ADD CONSTRAINT telemetry_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.telemetry_stream
    ADD CONSTRAINT telemetry_stream_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.time_scale_metrics
    ADD CONSTRAINT time_scale_metrics_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.token_rewards
    ADD CONSTRAINT token_rewards_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.tournament_mining_jobs
    ADD CONSTRAINT tournament_mining_jobs_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.tournament_participants
    ADD CONSTRAINT tournament_participants_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.tournament_sessions
    ADD CONSTRAINT tournament_sessions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.trading_positions
    ADD CONSTRAINT trading_positions_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.telemetry_stream
    ADD CONSTRAINT unique_active_stream UNIQUE (device_id, stream_status);

ALTER TABLE ONLY public.license_validations
    ADD CONSTRAINT unique_wallet_address UNIQUE (wallet_address);

ALTER TABLE ONLY public.user_achievements
    ADD CONSTRAINT user_achievements_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.user_engagement
    ADD CONSTRAINT user_engagement_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.user_learning_progress
    ADD CONSTRAINT user_learning_progress_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.user_onboarding
    ADD CONSTRAINT user_onboarding_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.user_onboarding
    ADD CONSTRAINT user_onboarding_user_id_tutorial_step_key UNIQUE (user_id, tutorial_step);

ALTER TABLE ONLY public.users
    ADD CONSTRAINT users_email_key UNIQUE (email);

ALTER TABLE ONLY public.users
    ADD CONSTRAINT users_pkey PRIMARY KEY (id);

ALTER TABLE ONLY public.valuation_history
    ADD CONSTRAINT valuation_history_pkey PRIMARY KEY (id);

CREATE INDEX idx_access_tokens_signature ON public.access_tokens USING btree (signature_id);

CREATE INDEX idx_access_tokens_token ON public.access_tokens USING btree (token);

CREATE INDEX idx_ads_status_dates ON public.ads USING btree (status, start_date, end_date);

CREATE INDEX idx_ads_zone_id ON public.ads USING btree (zone_id);

CREATE INDEX idx_alert_configs_user ON public.performance_alert_configs USING btree (user_id);

CREATE INDEX idx_alert_history_config ON public.performance_alert_history USING btree (alert_config_id);

CREATE INDEX idx_algo_backtests_user ON public.algo_backtests USING btree (user_id);

CREATE INDEX idx_algo_configurations_user ON public.algo_configurations USING btree (user_id);

CREATE INDEX idx_api_analysis_endpoint ON public.api_analysis USING btree (endpoint);

CREATE INDEX idx_api_analysis_timestamp ON public.api_analysis USING btree (analysis_timestamp);

CREATE INDEX idx_api_analysis_user ON public.api_analysis USING btree (user_id);

CREATE INDEX idx_blockchain_assets_contract ON public.blockchain_assets USING btree (contract_id);

CREATE INDEX idx_blockchain_assets_network ON public.blockchain_assets USING btree (network_id);

CREATE INDEX idx_blockchain_assets_owner ON public.blockchain_assets USING btree (owner_id);

CREATE INDEX idx_blockchain_transactions_contract ON public.blockchain_transactions USING btree (contract_id);

CREATE INDEX idx_blockchain_transactions_network ON public.blockchain_transactions USING btree (network_id);

CREATE INDEX idx_blockchain_transactions_user ON public.blockchain_transactions USING btree (user_id);

CREATE INDEX idx_club_announcements_club_id ON public.club_announcements USING btree (club_id);

CREATE INDEX idx_club_events_club_id ON public.club_events USING btree (club_id);

CREATE INDEX idx_club_events_date ON public.club_events USING btree (event_date);

CREATE INDEX idx_club_memberships_club_id ON public.club_memberships USING btree (club_id);

CREATE INDEX idx_club_memberships_tier_id ON public.club_memberships USING btree (tier_id);

CREATE INDEX idx_club_roles_club_user ON public.club_roles USING btree (club_id, user_id);

CREATE INDEX idx_consultancy_subscriptions_plan ON public.consultancy_subscriptions USING btree (plan_id);

CREATE INDEX idx_consultancy_subscriptions_user ON public.consultancy_subscriptions USING btree (user_id);

CREATE INDEX idx_content_waivers_content_id ON public.content_waivers USING btree (content_id);

CREATE INDEX idx_content_waivers_email ON public.content_waivers USING btree (email);

CREATE INDEX idx_content_waivers_user_id ON public.content_waivers USING btree (user_id);

CREATE INDEX idx_digital_signatures_hash ON public.digital_signatures USING btree (file_hash);

CREATE INDEX idx_digital_signatures_user ON public.digital_signatures USING btree (user_id);

CREATE INDEX idx_email_notifications_status ON public.email_notifications USING btree (status);

CREATE INDEX idx_email_notifications_type ON public.email_notifications USING btree (notification_type);

CREATE INDEX idx_email_notifications_user ON public.email_notifications USING btree (user_id);

CREATE INDEX idx_enhanced_metrics_player ON public.enhanced_performance_metrics USING btree (player_id);

CREATE INDEX idx_enhanced_metrics_timestamp ON public.enhanced_performance_metrics USING btree ("timestamp");

CREATE INDEX idx_event_registrations ON public.event_registrations USING btree (event_id, user_id);

CREATE INDEX idx_financial_scaling_team ON public.financial_scaling USING btree (team_id);

CREATE INDEX idx_impressions_ad_id ON public.ad_impressions USING btree (ad_id);

CREATE INDEX idx_impressions_timestamp ON public.ad_impressions USING btree ("timestamp");

CREATE INDEX idx_investment_indices_team ON public.investment_indices USING btree (team_id);

CREATE INDEX idx_investment_indices_tournament ON public.investment_indices USING btree (tournament_id);

CREATE INDEX idx_investment_portfolios_user ON public.investment_portfolios USING btree (user_id);

CREATE INDEX idx_investment_transactions_investment ON public.investment_transactions USING btree (investment_id);

CREATE INDEX idx_investments_portfolio ON public.investments USING btree (portfolio_id);

CREATE INDEX idx_license_validations_user ON public.license_validations USING btree (user_id);

CREATE INDEX idx_license_validations_user_expiry ON public.license_validations USING btree (user_id, expires_at);

CREATE INDEX idx_license_validations_wallet ON public.license_validations USING btree (wallet_address);

CREATE INDEX idx_market_analysis_timestamp ON public.market_analysis USING btree (analysis_timestamp);

CREATE INDEX idx_market_data_ticker ON public.market_data USING btree (ticker);

CREATE INDEX idx_market_data_timestamp ON public.market_data USING btree ("timestamp");

CREATE INDEX idx_metric_achievements_metric ON public.metric_achievements USING btree (time_scale_metric_id);

CREATE INDEX idx_metric_achievements_player ON public.metric_achievements USING btree (player_id);

CREATE INDEX idx_metric_market_correlations_scale ON public.metric_market_correlations USING btree (metric_type);

CREATE INDEX idx_ml_predictions_metric ON public.performance_ml_predictions USING btree (metric_id);

CREATE INDEX idx_mobile_metric_cache_expiry ON public.mobile_metric_cache USING btree (expires_at);

CREATE INDEX idx_mobile_metric_cache_lookup ON public.mobile_metric_cache USING btree (user_id, metric_type, cache_key);

CREATE INDEX idx_mobile_sessions_user_device ON public.mobile_sessions USING btree (user_id, device_id);

CREATE INDEX idx_onboarding_rewards_user ON public.onboarding_rewards USING btree (user_id);

CREATE INDEX idx_onboarding_settings_user ON public.onboarding_settings USING btree (user_id);

CREATE INDEX idx_payment_recovery_status ON public.payment_recovery_attempts USING btree (status);

CREATE INDEX idx_payment_recovery_sub_id ON public.payment_recovery_attempts USING btree (subscription_id);

CREATE INDEX idx_performance_metrics_match_date ON public.performance_metrics USING btree (match_date);

CREATE INDEX idx_performance_metrics_player ON public.performance_metrics USING btree (player_id);

CREATE INDEX idx_performance_metrics_session ON public.performance_metrics USING btree (session_date);

CREATE INDEX idx_performance_metrics_sport ON public.performance_metrics USING btree (sport_type);

CREATE INDEX idx_performance_predictions_session ON public.performance_predictions USING btree (tournament_session_id);

CREATE INDEX idx_performance_trends_metric ON public.performance_trends USING btree (metric_type);

CREATE INDEX idx_performance_yields_user_date ON public.performance_yields USING btree (user_id, earned_date);

CREATE INDEX idx_player_devices_device_id ON public.player_devices USING btree (device_id);

CREATE INDEX idx_player_devices_player_id ON public.player_devices USING btree (player_id);

CREATE INDEX idx_player_heatmaps_performance_metric ON public.player_heatmaps USING btree (performance_metric_id);

CREATE INDEX idx_players_club_id ON public.players USING btree (club_id);

CREATE INDEX idx_players_team_id ON public.players USING btree (team_id);

CREATE INDEX idx_players_user_id ON public.players USING btree (user_id);

CREATE INDEX idx_playtime_correlations_player ON public.playtime_correlations USING btree (player_id);

CREATE INDEX idx_playtime_metrics_date ON public.playtime_metrics USING btree (session_date);

CREATE INDEX idx_playtime_metrics_player ON public.playtime_metrics USING btree (player_id);

CREATE INDEX idx_playtime_thresholds_sport ON public.playtime_thresholds USING btree (sport_type, intensity_level);

CREATE INDEX idx_position_history_entry_date ON public.position_history USING btree (entry_date);

CREATE INDEX idx_position_history_exit_date ON public.position_history USING btree (exit_date);

CREATE INDEX idx_position_history_ticker ON public.position_history USING btree (ticker);

CREATE INDEX idx_position_history_user_id ON public.position_history USING btree (user_id);

CREATE INDEX idx_request_tracking_endpoint ON public.request_tracking USING btree (endpoint);

CREATE INDEX idx_request_tracking_ip ON public.request_tracking USING btree (ip_address);

CREATE INDEX idx_revenue_transactions_club_id ON public.revenue_transactions USING btree (club_id);

CREATE INDEX idx_revenue_transactions_created_at ON public.revenue_transactions USING btree (created_at);

CREATE INDEX idx_security_events_type ON public.security_events USING btree (event_type);

CREATE INDEX idx_security_events_user_id ON public.security_events USING btree (user_id);

CREATE INDEX idx_sleep_mode_analytics_event_type ON public.sleep_mode_analytics USING btree (event_type);

CREATE INDEX idx_sleep_mode_analytics_timestamp ON public.sleep_mode_analytics USING btree (event_timestamp);

CREATE INDEX idx_sleep_mode_analytics_user ON public.sleep_mode_analytics USING btree (user_id);

CREATE INDEX idx_smart_ball_devices_user ON public.smart_ball_devices USING btree (user_id);

CREATE INDEX idx_smart_ball_events_device ON public.smart_ball_events USING btree (device_id);

CREATE INDEX idx_smart_ball_events_device_id ON public.smart_ball_events USING btree (device_id);

CREATE INDEX idx_smart_ball_events_timestamp ON public.smart_ball_events USING btree ("timestamp");

CREATE INDEX idx_smart_ball_events_tournament ON public.smart_ball_events USING btree (tournament_id);

CREATE INDEX idx_smart_contracts_network ON public.smart_contracts USING btree (network_id);

CREATE INDEX idx_store_orders_status ON public.store_orders USING btree (status);

CREATE INDEX idx_store_orders_user ON public.store_orders USING btree (user_id);

CREATE INDEX idx_subscription_analytics_period ON public.subscription_analytics USING btree (period_start, period_end);

CREATE INDEX idx_subscription_analytics_sub_id ON public.subscription_analytics USING btree (subscription_id);

CREATE INDEX idx_subscription_payments_subscription_id ON public.subscription_payments USING btree (subscription_id);

CREATE INDEX idx_subscription_payments_user_id ON public.subscription_payments USING btree (user_id);

CREATE INDEX idx_subscription_usage_feature ON public.subscription_usage USING btree (feature_name);

CREATE INDEX idx_subscription_usage_sub_id ON public.subscription_usage USING btree (subscription_id);

CREATE INDEX idx_subscriptions_status ON public.subscriptions USING btree (status);

CREATE INDEX idx_subscriptions_user_id ON public.subscriptions USING btree (user_id);

CREATE INDEX idx_telemetry_active_streams ON public.telemetry_stream USING btree (device_id) WHERE ((stream_status)::text = 'active'::text);

CREATE INDEX idx_telemetry_metrics_stream ON public.telemetry_metrics USING btree (stream_id, collected_at);

CREATE INDEX idx_telemetry_session ON public.telemetry_data USING btree (session_id);

CREATE INDEX idx_telemetry_user_timestamp ON public.telemetry_data USING btree (user_id, "timestamp");

CREATE INDEX idx_time_scale_metrics_scale ON public.time_scale_metrics USING btree (time_scale);

CREATE INDEX idx_token_rewards_metric ON public.token_rewards USING btree (metric_id);

CREATE INDEX idx_tournament_jobs_dates ON public.tournament_mining_jobs USING btree (start_date, end_date);

CREATE INDEX idx_tournament_jobs_status ON public.tournament_mining_jobs USING btree (status);

CREATE INDEX idx_tournament_participants_user ON public.tournament_participants USING btree (user_id);

CREATE INDEX idx_tournament_sessions_status ON public.tournament_sessions USING btree (status);

CREATE INDEX idx_tournament_sessions_tournament ON public.tournament_sessions USING btree (tournament_id);

CREATE INDEX idx_tournament_sessions_user ON public.tournament_sessions USING btree (user_id);

CREATE INDEX idx_trading_positions_status ON public.trading_positions USING btree (status);

CREATE INDEX idx_trading_positions_user_date ON public.trading_positions USING btree (user_id, entry_date);

CREATE INDEX idx_trading_positions_user_id ON public.trading_positions USING btree (user_id);

CREATE INDEX idx_user_achievements_user ON public.user_achievements USING btree (user_id);

CREATE INDEX idx_user_engagement_user ON public.user_engagement USING btree (user_id);

CREATE INDEX idx_user_learning_progress_content_id ON public.user_learning_progress USING btree (content_id);

CREATE INDEX idx_user_learning_progress_user_id ON public.user_learning_progress USING btree (user_id);

CREATE INDEX idx_user_onboarding_user ON public.user_onboarding USING btree (user_id);

ALTER TABLE ONLY public.access_tokens
    ADD CONSTRAINT access_tokens_signature_id_fkey FOREIGN KEY (signature_id) REFERENCES public.digital_signatures(id);

ALTER TABLE ONLY public.ad_impressions
    ADD CONSTRAINT ad_impressions_ad_id_fkey FOREIGN KEY (ad_id) REFERENCES public.ads(id);

ALTER TABLE ONLY public.ad_impressions
    ADD CONSTRAINT ad_impressions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.ads
    ADD CONSTRAINT ads_zone_id_fkey FOREIGN KEY (zone_id) REFERENCES public.ad_zones(id);

ALTER TABLE ONLY public.advanced_metric_correlations
    ADD CONSTRAINT advanced_metric_correlations_primary_metric_id_fkey FOREIGN KEY (primary_metric_id) REFERENCES public.enhanced_performance_metrics(id);

ALTER TABLE ONLY public.advanced_metric_correlations
    ADD CONSTRAINT advanced_metric_correlations_secondary_metric_id_fkey FOREIGN KEY (secondary_metric_id) REFERENCES public.enhanced_performance_metrics(id);

ALTER TABLE ONLY public.algo_backtests
    ADD CONSTRAINT algo_backtests_configuration_id_fkey FOREIGN KEY (configuration_id) REFERENCES public.algo_configurations(id);

ALTER TABLE ONLY public.algo_backtests
    ADD CONSTRAINT algo_backtests_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.algo_configurations
    ADD CONSTRAINT algo_configurations_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.api_analysis
    ADD CONSTRAINT api_analysis_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.auth_accounts
    ADD CONSTRAINT "auth_accounts_userId_fkey" FOREIGN KEY ("userId") REFERENCES public.auth_users(id) ON DELETE CASCADE;

ALTER TABLE ONLY public.auth_sessions
    ADD CONSTRAINT "auth_sessions_userId_fkey" FOREIGN KEY ("userId") REFERENCES public.auth_users(id) ON DELETE CASCADE;

ALTER TABLE ONLY public.blockchain_assets
    ADD CONSTRAINT blockchain_assets_contract_id_fkey FOREIGN KEY (contract_id) REFERENCES public.smart_contracts(id);

ALTER TABLE ONLY public.blockchain_assets
    ADD CONSTRAINT blockchain_assets_network_id_fkey FOREIGN KEY (network_id) REFERENCES public.blockchain_networks(id);

ALTER TABLE ONLY public.blockchain_assets
    ADD CONSTRAINT blockchain_assets_owner_id_fkey FOREIGN KEY (owner_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.blockchain_transactions
    ADD CONSTRAINT blockchain_transactions_contract_id_fkey FOREIGN KEY (contract_id) REFERENCES public.smart_contracts(id);

ALTER TABLE ONLY public.blockchain_transactions
    ADD CONSTRAINT blockchain_transactions_network_id_fkey FOREIGN KEY (network_id) REFERENCES public.blockchain_networks(id);

ALTER TABLE ONLY public.blockchain_transactions
    ADD CONSTRAINT blockchain_transactions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.budget_goals
    ADD CONSTRAINT budget_goals_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.club_announcements
    ADD CONSTRAINT club_announcements_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.club_events
    ADD CONSTRAINT club_events_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.club_memberships
    ADD CONSTRAINT club_memberships_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.club_memberships
    ADD CONSTRAINT club_memberships_tier_id_fkey FOREIGN KEY (tier_id) REFERENCES public.membership_tiers(id);

ALTER TABLE ONLY public.club_roles
    ADD CONSTRAINT club_roles_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.club_roles
    ADD CONSTRAINT club_roles_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.connected_devices
    ADD CONSTRAINT connected_devices_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.consultancy_service_features
    ADD CONSTRAINT consultancy_service_features_plan_id_fkey FOREIGN KEY (plan_id) REFERENCES public.consultancy_plans(id);

ALTER TABLE ONLY public.consultancy_subscriptions
    ADD CONSTRAINT consultancy_subscriptions_plan_id_fkey FOREIGN KEY (plan_id) REFERENCES public.consultancy_plans(id);

ALTER TABLE ONLY public.consultancy_subscriptions
    ADD CONSTRAINT consultancy_subscriptions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.content_waivers
    ADD CONSTRAINT content_waivers_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.customer_subscriptions
    ADD CONSTRAINT customer_subscriptions_plan_id_fkey FOREIGN KEY (plan_id) REFERENCES public.subscription_plans(id);

ALTER TABLE ONLY public.customer_subscriptions
    ADD CONSTRAINT customer_subscriptions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.digital_signatures
    ADD CONSTRAINT digital_signatures_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.email_notifications
    ADD CONSTRAINT email_notifications_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.email_preferences
    ADD CONSTRAINT email_preferences_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.enhanced_performance_metrics
    ADD CONSTRAINT enhanced_performance_metrics_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.event_registrations
    ADD CONSTRAINT event_registrations_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.club_events(id);

ALTER TABLE ONLY public.event_registrations
    ADD CONSTRAINT event_registrations_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.financial_scaling
    ADD CONSTRAINT financial_scaling_team_id_fkey FOREIGN KEY (team_id) REFERENCES public.teams(id);

ALTER TABLE ONLY public.investment_indices
    ADD CONSTRAINT investment_indices_team_id_fkey FOREIGN KEY (team_id) REFERENCES public.teams(id);

ALTER TABLE ONLY public.investment_indices
    ADD CONSTRAINT investment_indices_tournament_id_fkey FOREIGN KEY (tournament_id) REFERENCES public.tournament_mining_jobs(id);

ALTER TABLE ONLY public.investment_portfolios
    ADD CONSTRAINT investment_portfolios_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.investment_transactions
    ADD CONSTRAINT investment_transactions_investment_id_fkey FOREIGN KEY (investment_id) REFERENCES public.investments(id);

ALTER TABLE ONLY public.investments
    ADD CONSTRAINT investments_portfolio_id_fkey FOREIGN KEY (portfolio_id) REFERENCES public.investment_portfolios(id) ON DELETE CASCADE;

ALTER TABLE ONLY public.learn_progress
    ADD CONSTRAINT learn_progress_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.learning_content
    ADD CONSTRAINT learning_content_parent_section_fkey FOREIGN KEY (parent_section) REFERENCES public.learning_content(id);

ALTER TABLE ONLY public.license_validations
    ADD CONSTRAINT license_validations_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.market_analysis
    ADD CONSTRAINT market_analysis_performance_metric_id_fkey FOREIGN KEY (performance_metric_id) REFERENCES public.performance_metrics(id);

ALTER TABLE ONLY public.metric_achievements
    ADD CONSTRAINT metric_achievements_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.metric_achievements
    ADD CONSTRAINT metric_achievements_time_scale_metric_id_fkey FOREIGN KEY (time_scale_metric_id) REFERENCES public.time_scale_metrics(id);

ALTER TABLE ONLY public.mobile_app_settings
    ADD CONSTRAINT mobile_app_settings_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.mobile_metric_cache
    ADD CONSTRAINT mobile_metric_cache_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.mobile_sessions
    ADD CONSTRAINT mobile_sessions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.onboarding_rewards
    ADD CONSTRAINT onboarding_rewards_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.onboarding_settings
    ADD CONSTRAINT onboarding_settings_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.payment_history
    ADD CONSTRAINT payment_history_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.customer_subscriptions(id);

ALTER TABLE ONLY public.payment_recovery_attempts
    ADD CONSTRAINT payment_recovery_attempts_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.consultancy_subscriptions(id);

ALTER TABLE ONLY public.performance_alert_configs
    ADD CONSTRAINT performance_alert_configs_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.performance_alert_history
    ADD CONSTRAINT performance_alert_history_alert_config_id_fkey FOREIGN KEY (alert_config_id) REFERENCES public.performance_alert_configs(id);

ALTER TABLE ONLY public.performance_metrics_history
    ADD CONSTRAINT performance_metrics_history_metric_id_fkey FOREIGN KEY (metric_id) REFERENCES public.performance_metrics(id);

ALTER TABLE ONLY public.performance_metrics
    ADD CONSTRAINT performance_metrics_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.performance_ml_predictions
    ADD CONSTRAINT performance_ml_predictions_metric_id_fkey FOREIGN KEY (metric_id) REFERENCES public.enhanced_performance_metrics(id);

ALTER TABLE ONLY public.performance_predictions
    ADD CONSTRAINT performance_predictions_tournament_session_id_fkey FOREIGN KEY (tournament_session_id) REFERENCES public.tournament_sessions(id);

ALTER TABLE ONLY public.performance_spending_analytics
    ADD CONSTRAINT performance_spending_analytics_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.performance_yields
    ADD CONSTRAINT performance_yields_performance_metric_id_fkey FOREIGN KEY (performance_metric_id) REFERENCES public.performance_metrics(id);

ALTER TABLE ONLY public.performance_yields
    ADD CONSTRAINT performance_yields_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.player_devices
    ADD CONSTRAINT player_devices_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.players(id);

ALTER TABLE ONLY public.player_heatmaps
    ADD CONSTRAINT player_heatmaps_performance_metric_id_fkey FOREIGN KEY (performance_metric_id) REFERENCES public.performance_metrics(id);

ALTER TABLE ONLY public.players
    ADD CONSTRAINT players_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.players
    ADD CONSTRAINT players_team_id_fkey FOREIGN KEY (team_id) REFERENCES public.teams(id);

ALTER TABLE ONLY public.players
    ADD CONSTRAINT players_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.playtime_correlations
    ADD CONSTRAINT playtime_correlations_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.playtime_metrics
    ADD CONSTRAINT playtime_metrics_player_id_fkey FOREIGN KEY (player_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.position_history
    ADD CONSTRAINT position_history_position_id_fkey FOREIGN KEY (position_id) REFERENCES public.trading_positions(id);

ALTER TABLE ONLY public.position_history
    ADD CONSTRAINT position_history_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.revenue_transactions
    ADD CONSTRAINT revenue_transactions_club_id_fkey FOREIGN KEY (club_id) REFERENCES public.clubs(id);

ALTER TABLE ONLY public.security_events
    ADD CONSTRAINT security_events_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.simulation_logs
    ADD CONSTRAINT simulation_logs_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.sleep_mode_analytics
    ADD CONSTRAINT sleep_mode_analytics_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.sleep_mode_analytics
    ADD CONSTRAINT sleep_mode_analytics_variant_id_fkey FOREIGN KEY (variant_id) REFERENCES public.sleep_mode_test_variants(id);

ALTER TABLE ONLY public.sleep_mode_user_variants
    ADD CONSTRAINT sleep_mode_user_variants_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.sleep_mode_user_variants
    ADD CONSTRAINT sleep_mode_user_variants_variant_id_fkey FOREIGN KEY (variant_id) REFERENCES public.sleep_mode_test_variants(id);

ALTER TABLE ONLY public.smart_ball_devices
    ADD CONSTRAINT smart_ball_devices_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.smart_ball_events
    ADD CONSTRAINT smart_ball_events_device_id_fkey FOREIGN KEY (device_id) REFERENCES public.smart_ball_devices(id);

ALTER TABLE ONLY public.smart_contracts
    ADD CONSTRAINT smart_contracts_network_id_fkey FOREIGN KEY (network_id) REFERENCES public.blockchain_networks(id);

ALTER TABLE ONLY public.spending_tracker
    ADD CONSTRAINT spending_tracker_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.sponsorship_tokens
    ADD CONSTRAINT sponsorship_tokens_athlete_id_fkey FOREIGN KEY (athlete_id) REFERENCES public.players(id);

ALTER TABLE ONLY public.store_order_items
    ADD CONSTRAINT store_order_items_order_id_fkey FOREIGN KEY (order_id) REFERENCES public.store_orders(id);

ALTER TABLE ONLY public.store_order_items
    ADD CONSTRAINT store_order_items_product_id_fkey FOREIGN KEY (product_id) REFERENCES public.store_products(id);

ALTER TABLE ONLY public.store_orders
    ADD CONSTRAINT store_orders_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.subscription_analytics
    ADD CONSTRAINT subscription_analytics_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.consultancy_subscriptions(id);

ALTER TABLE ONLY public.subscription_features
    ADD CONSTRAINT subscription_features_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.subscriptions(id);

ALTER TABLE ONLY public.subscription_payments
    ADD CONSTRAINT subscription_payments_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.subscriptions(id);

ALTER TABLE ONLY public.subscription_payments
    ADD CONSTRAINT subscription_payments_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.subscription_usage
    ADD CONSTRAINT subscription_usage_subscription_id_fkey FOREIGN KEY (subscription_id) REFERENCES public.consultancy_subscriptions(id);

ALTER TABLE ONLY public.subscriptions
    ADD CONSTRAINT subscriptions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.telemetry_data
    ADD CONSTRAINT telemetry_data_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.telemetry_stream
    ADD CONSTRAINT telemetry_stream_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.token_rewards
    ADD CONSTRAINT token_rewards_metric_id_fkey FOREIGN KEY (metric_id) REFERENCES public.performance_metrics(id);

ALTER TABLE ONLY public.tournament_participants
    ADD CONSTRAINT tournament_participants_device_id_fkey FOREIGN KEY (device_id) REFERENCES public.smart_ball_devices(id);

ALTER TABLE ONLY public.tournament_participants
    ADD CONSTRAINT tournament_participants_tournament_id_fkey FOREIGN KEY (tournament_id) REFERENCES public.tournament_mining_jobs(id);

ALTER TABLE ONLY public.tournament_participants
    ADD CONSTRAINT tournament_participants_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.tournament_sessions
    ADD CONSTRAINT tournament_sessions_device_id_fkey FOREIGN KEY (device_id) REFERENCES public.smart_ball_devices(id);

ALTER TABLE ONLY public.tournament_sessions
    ADD CONSTRAINT tournament_sessions_tournament_id_fkey FOREIGN KEY (tournament_id) REFERENCES public.tournament_mining_jobs(id);

ALTER TABLE ONLY public.tournament_sessions
    ADD CONSTRAINT tournament_sessions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.trading_positions
    ADD CONSTRAINT trading_positions_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id) ON DELETE CASCADE;

ALTER TABLE ONLY public.user_achievements
    ADD CONSTRAINT user_achievements_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.user_engagement
    ADD CONSTRAINT user_engagement_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.user_learning_progress
    ADD CONSTRAINT user_learning_progress_content_id_fkey FOREIGN KEY (content_id) REFERENCES public.learning_content(id);

ALTER TABLE ONLY public.user_learning_progress
    ADD CONSTRAINT user_learning_progress_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.users(id);

ALTER TABLE ONLY public.user_onboarding
    ADD CONSTRAINT user_onboarding_user_id_fkey FOREIGN KEY (user_id) REFERENCES public.auth_users(id);

ALTER TABLE ONLY public.valuation_history
    ADD CONSTRAINT valuation_history_team_id_fkey FOREIGN KEY (team_id) REFERENCES public.teams(id);

ALTER DEFAULT PRIVILEGES FOR ROLE cloud_admin IN SCHEMA public GRANT ALL ON SEQUENCES TO neon_superuser WITH GRANT OPTION
