
SELECT
     COUNT(DISTINCT order_id) FILTER (WHERE action != 'deliver_order') as orders_undelivered,
     COUNT(DISTINCT order_id) as orders_canceled,
     COUNT(DISTINCT order_id) FILTER(WHERE order_id IN (SELECT order_id
                                                           FROM courier_actions
                                                           WHERE action = 'deliver_order')) AS orders_in_process
FROM courier_actions
WHERE order_id in (SELECT order_id
                   FROM user_actions
                   WHERE action = 'cancel_order')