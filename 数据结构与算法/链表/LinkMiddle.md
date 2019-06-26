package com.crossoverjie.algorithm;

/**
 * 问题
 * <p>
 * 给出任意单向链表，找出并返回该链表的中间节点。
 * <p>
 * 奇数长度的链表，例如：1->2->3->4->5 返回节点 3
 * <p>
 * 偶长度的链表，例如：1->2->3->4->5->6 返回节点 4
 * <p>
 * 解题思路
 * <p>
 * 与链表中环的检测一样，这题同样可以使用快慢指针。
 * <p>
 * 定义两个指针fast和slow。slow一次遍历一个节点，fast一次遍历两个节点，由于fast的速度是slow的两倍，所以当fast遍历完链表时，slow所处的节点就是链表的中间节点。
 */
public class LinkMiddle {

    public class ListNode {
        int val;
        ListNode next;

        ListNode(int x) {
            val = x;
        }
    }

    public ListNode middleNode(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
