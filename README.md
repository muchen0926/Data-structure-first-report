# 資料結構 期中報告
11024219 任茂佶
# 期中報告第三題
Python實現的單向循環鍊表功能示例
## 概述
單向循環鍊錶是指在單鍊錶的基礎上，表的最後一個元素指向鍊錶頭結點，不再是為空。



由圖可知，單向循環鍊錶的判斷條件不再是表為空了，而變成了是否到表頭。

## 操作

is_empty() 判斷鍊錶是否為空

length() 傳回鍊錶的長度

travel() 遍歷

add(item) 在頭部新增一個節點

append(item) 在尾部增加一個節點

insert(pos, item) 在指定位置pos新增節點

remove(item) 刪除一個節點

search(item) 查找節點是否存在

## 具體代碼:

    class Node(object):
    """ 節點 """
    def __init__(self, item):
        self.item = item
        self.next = None

    class SinCycLinkedlist(object):
    """單向循環鏈表"""
    def __init__(self):
        self._head = None

    def is_empty(self):
        """判斷鏈表是否為空"""
        return self._head == None

    def length(self):
        """返回鏈表的長度"""  
        # 如果鏈表為空,返回長度0
        if self.is_empty():
            return 0
        count = 1
        cur = self._head
        while cur.next != self._head:
            count += 1
            cur = cur.next
        return count

    def travel(self):
        """遍歷鏈表"""
        if self.is_empty():
            return
        cur = self._head
        print(cur.item, end=" ")
        while cur.next != self._head:
            cur = cur.next
            print(cur.item, end=" ")
        print("")

    def add(self, item):
        """頭部添加節點"""
        node = Node(item)
        if self.is_empty():
            self._head = node
            node.next = self._head
        else:
            # 添加的節點指向_head
            node.next = self._head
            # 移到鏈表尾部,將尾部節點的next指向node
            cur = self._head
            while cur.next != self._head:
                cur = cur.next
            cur.next = node
            # _head指向添加node的
            self._head = node

    def append(self, item):
        """尾部添加節點"""
        node = Node(item)
        if self.is_empty():
            self._head = node
            node.next = self._head
        else:
            # 移到鏈表尾部
            cur = self._head
            while cur.next != self._head:
                cur = cur.next
            # 將尾節點指向node
            cur.next = node
            # 將node指向頭節點_head
            node.next = self._head

    def insert(self, pos, item):
        """在指定位置添加節點"""
        if pos <= 0:
            self.add(item)
        elif pos > (self.length()-1):
            self.append(item)
        else:
            node = Node(item)
            cur = self._head
            count = 0
            # 移動到指定位置的前一個位置
            while count < (pos-1):
                count += 1
                cur = cur.next
            node.next = cur.next
            cur.next = node

    def remove(self, item):
        """刪除一個節點"""
        # 若鏈表為空,則直接返回
        if self.is_empty():
            return
        # 將cur指向頭節點
        cur = self._head
        pre = None
        # 若頭節點的元素就是要查找的元素item
        if cur.item == item:
            # 如果鏈表不止一個節點
            if cur.next != self._head:
                # 先找到尾節點,將尾節點的next指向第二個節點
                while cur.next != self._head:
                    cur = cur.next
                # cur指向了尾節點
                cur.next = self._head.next
                self._head = self._head.next
            else:
                # 鏈表只有一個節點
                self._head = None
        else:
            pre = self._head
            # 第一個節點不是要刪除的
            while cur.next != self._head:
                # 找到了要刪除的元素
                if cur.item == item:
                    # 刪除
                    pre.next = cur.next
                    return
                else:
                    pre = cur
                    cur = cur.next
            # cur 指向尾節點
            if cur.item == item:
                # 尾部刪除
                pre.next = cur.next

    def search(self, item):
        """查找節點是否存在"""
        if self.is_empty():
            return False
        cur = self._head
        if cur.item == item:
            return True
        cur = cur.next
        while cur != self._head:
            if cur.item == item:
                return True
            cur = cur.next
        return False

    if __name__ == "__main__":
        ll = SinCycLinkedlist()
        ll.add(1)
        ll.add(2)
        ll.append(3)
        ll.insert(2, 4)
        ll.insert(4, 5)
        ll.insert(0, 6)
        print("length:", ll.length())
        ll.travel()
        print(ll.search(3))
        print(ll.search(7))
        ll.remove(1)
        print("length:", ll.length())
        ll.travel()
