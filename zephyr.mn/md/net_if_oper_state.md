```c
enum net_if_oper_state {
	NET_IF_OPER_UNKNOWN,        /**< Initial (unknown) value */
	NET_IF_OPER_NOTPRESENT,     /**< Hardware missing */
	NET_IF_OPER_DOWN,           /**< Interface is down */
	NET_IF_OPER_LOWERLAYERDOWN, /**< Lower layer interface is down */
	NET_IF_OPER_TESTING,        /**< Training mode */
	NET_IF_OPER_DORMANT,        /**< Waiting external action */
	NET_IF_OPER_UP,             /**< Interface is up */
} __packed;
```
