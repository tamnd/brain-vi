---
title: "CF 102878E - Chuỗi con riêng"
description: "Chúng ta được cấp một chuỗi chữ thường s. Một chuỗi con của một chuỗi được gọi là chuỗi con riêng khi chuỗi ký tự chính xác đó chỉ xuất hiện một lần trong chuỗi. Nhiệm vụ là xem xét mọi tiền tố của s và tìm độ dài của chuỗi con riêng ngắn nhất bên trong tiền tố đó."
date: "2026-07-25T12:43:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "E"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 41
verified: true
draft: false
---

[CF 102878E - Chuỗi con Eigen](https://codeforces.com/problemset/problem/102878/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ thường`s`. Một chuỗi con của một chuỗi được gọi là chuỗi con riêng khi chuỗi ký tự chính xác đó chỉ xuất hiện một lần trong chuỗi. Nhiệm vụ là xem xét mọi tiền tố của`s`và tìm độ dài của chuỗi con riêng ngắn nhất bên trong tiền tố đó. 

Ví dụ: khi tiền tố là`ababb`, chuỗi con`b`không hợp lệ vì nó xuất hiện nhiều lần, nhưng`ba`hợp lệ vì nó chỉ xảy ra một lần. Câu trả lời cho tiền tố này là`2`. 

Đầu vào chứa độ dài của chuỗi và chính chuỗi đó. Đầu ra chứa một số cho mỗi tiền tố, trong đó số này mô tả chuỗi con ngắn nhất xuất hiện chính xác một lần trong tiền tố đó. 

Chiều dài của chuỗi có thể đạt tới`10^6`. Giải pháp liệt kê các chuỗi con không thể hoạt động vì một chuỗi có kích thước này có khoảng`n²/2`các chuỗi con. Ngay cả việc lưu trữ và kiểm tra mọi chuỗi con có thể cũng cần khoảng`10^12`hoạt động. Giải pháp phải xử lý mỗi ký tự một số lần không đổi hoặc logarit. 

Những trường hợp khó khăn không chỉ là những chuỗi dài. Các mô hình lặp đi lặp lại là nơi mà các giải pháp sai thường thất bại. Một chuỗi con duy nhất có thể không còn là duy nhất sau khi thêm nhiều ký tự hơn và một chuỗi con không phải là duy nhất có thể trở nên không liên quan vì chuỗi con dài hơn sẽ trở thành duy nhất. 

Ví dụ:```
1
a
```Câu trả lời là:```
1
```Chuỗi con duy nhất là`a`, và nó xuất hiện một lần. 

Một ví dụ khác:```
4
aaaa
```Câu trả lời là:```
1
2
3
4
```Sau ký tự đầu tiên,`a`là duy nhất Sau ký tự thứ hai,`a`xuất hiện hai lần, vì vậy`aa`là chuỗi con duy nhất ngắn nhất. Lý do tương tự tiếp tục khi chuỗi phát triển. Giải pháp chỉ kiểm tra hậu tố mới nhất sẽ bỏ sót các chuỗi con trước đó có thể mất tính duy nhất. 

Trường hợp quan trọng cuối cùng là một chuỗi có nhiều ký tự khác nhau:```
5
abcde
```Câu trả lời là:```
1
1
1
1
1
```Mỗi ký tự đơn lẻ đều là duy nhất, vì vậy các chuỗi con dài hơn sẽ không bao giờ thay thế chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tạo ra mọi chuỗi con của tiền tố hiện tại, đếm số lần mỗi chuỗi xuất hiện và giữ chuỗi ngắn nhất có số đếm một. Điều này đúng vì nó tuân theo định nghĩa chính xác. Vấn đề là chi phí. Đối với tiền tố có độ dài`n`, có`O(n²)`chuỗi con và việc kiểm tra sự xuất hiện của chúng có thể bổ sung thêm một yếu tố khác. Trường hợp xấu nhất trở nên vượt xa những gì có thể xảy ra`n = 10^6`. 

Điều quan trọng là các chuỗi con không cần phải được lưu trữ riêng lẻ. Một hậu tố tự động nhóm các chuỗi con có hành vi xuất hiện giống hệt nhau. Mỗi trạng thái đại diện cho một khoảng độ dài chuỗi con. Nếu một trạng thái tương ứng với các chuỗi con xuất hiện một lần thì mỗi độ dài trong khoảng đó cũng là duy nhất. Ứng cử viên thấp nhất được đại diện bởi một bang là`link_length + 1`. 

Máy tự động có thể được xây dựng trong khi quét chuỗi từ trái sang phải. Trong quá trình xây dựng, chúng tôi duy trì các trạng thái có chuỗi con được biểu thị hiện là duy nhất. Khi thêm một ký tự, một số trạng thái trở thành không duy nhất và phải bị xóa, trong khi trạng thái mới được tạo bởi tiện ích mở rộng sẽ trở thành một ứng cử viên. Giá trị tối thiểu trong số các ứng cử viên còn lại là câu trả lời cho tiền tố hiện tại. 

Lý do điều này hoạt động trực tuyến là vì trạng thái tự động hóa hậu tố thay đổi từ duy nhất sang không duy nhất chính xác khi một lần xuất hiện khác đạt đến cùng một lớp tương đương. Quá trình xây dựng đã bộc lộ những thay đổi đó khi đi theo các liên kết hậu tố và xử lý các bản sao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Bảo trì Automaton Suffix | O(n log n) với tập có thứ tự | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố trong khi đọc từng chuỗi một ký tự. Mỗi lần chèn thể hiện việc thêm ký tự hiện tại vào tiền tố hiện tại. 
2. Bất cứ khi nào một trạng thái máy tự động mới được tạo, hãy chèn độ dài chuỗi con được biểu thị ngắn nhất của nó vào cấu trúc ứng cử viên. Trạng thái đại diện cho một họ chuỗi con và thành viên ngắn nhất trong họ đó là độ dài duy nhất có thể cải thiện câu trả lời. 
3. Khi phần chèn sửa đổi một trạng thái hiện có vì số lần xuất hiện của nó không còn là một, hãy xóa trạng thái đó khỏi cấu trúc ứng cử viên. Một trạng thái có nhiều lần xuất hiện không thể đóng góp một chuỗi con riêng. 
4. Nếu trạng thái bản sao được tạo trong quá trình xây dựng máy tự động hậu tố, hãy cập nhật cấu trúc ứng cử viên cho trạng thái ban đầu bị ảnh hưởng vì bản sao chia tách khoảng chuỗi con được biểu thị. 
5. Sau khi xử lý từng ký tự, độ dài được lưu trữ nhỏ nhất là câu trả lời cho tiền tố hiện tại. 

Điều bất biến là cấu trúc ứng cử viên chứa chính xác độ dài ngắn nhất của tất cả các trạng thái ô tô hậu tố có chuỗi con được biểu thị xuất hiện một lần trong tiền tố hiện tại. Mọi chuỗi con riêng có thể thuộc về một trong các trạng thái này và mọi ứng cử viên được lưu trữ đều là một chuỗi con riêng. Do đó, lấy mức tối thiểu sẽ cho độ dài ngắn nhất chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n = int(input())
    s = input().strip()

    nxt = [dict()]
    link = [-1]
    length = [0]

    unique = [False]
    heap = []

    def add_candidate(v):
        if unique[v]:
            heapq.heappush(heap, (length[link[v]] + 1, v))

    def extend(c, last):
        cur = len(nxt)
        nxt.append({})
        length.append(length[last] + 1)
        link.append(0)
        unique.append(True)

        p = last
        while p != -1 and c not in nxt[p]:
            nxt[p][c] = cur
            p = link[p]

        if p == -1:
            link[cur] = 0
        else:
            q = nxt[p][c]
            if length[p] + 1 == length[q]:
                link[cur] = q
                if unique[q]:
                    unique[q] = False
            else:
                clone = len(nxt)
                nxt.append(nxt[q].copy())
                length.append(length[p] + 1)
                link.append(link[q])
                unique.append(unique[q])

                while p != -1 and nxt[p].get(c) == q:
                    nxt[p][c] = clone
                    p = link[p]

                link[q] = clone
                link[cur] = clone

                unique[q] = False
                unique[clone] = False

        add_candidate(cur)
        return cur

    last = 0
    for i, ch in enumerate(s):
        last = extend(ch, last)

        while heap:
            val, state = heap[0]
            if unique[state]:
                break
            heapq.heappop(heap)

        print(heap[0][0])

if __name__ == "__main__":
    solve()
```Mảng tự động lưu trữ bản đồ chuyển tiếp, liên kết hậu tố và độ dài tối đa cho mọi trạng thái. các`unique`mảng biểu thị liệu một trạng thái hiện có thể tạo ra chuỗi con riêng hay không. 

Heap lưu trữ các câu trả lời có thể có. Tính năng xóa lười được sử dụng vì nhiều trạng thái có thể trở nên không hợp lệ sau khi chèn. Thay vì tìm kiếm và xóa ngay lập tức mọi giá trị cũ, các mục nhập không hợp lệ chỉ bị xóa khi chúng đạt đến vị trí tối thiểu. 

biểu hiện`length[link[v]] + 1`là độ dài chuỗi con nhỏ nhất được biểu thị bằng trạng thái`v`. Bản thân trạng thái có thể biểu thị nhiều độ dài khác nhau, nhưng đây là giá trị duy nhất có thể cải thiện câu trả lời hiện tại. 

Việc xử lý bản sao là phần tế nhị nhất trong quá trình thực hiện. Một bản sao chia tách một lớp tương đương chuỗi con, do đó thông tin về tính duy nhất trước đó không thể được sao chép một cách đơn giản mà không cần điều chỉnh. 

(Phần 2 tiếp tục với các ví dụ đã hoạt động, chi tiết phức tạp và bộ thử nghiệm hoàn chỉnh.)
