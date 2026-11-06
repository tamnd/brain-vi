---
title: "CF 1255B - Tủ Lạnh"
description: "Chúng tôi được cung cấp một hệ thống tủ khóa hoàn chỉnh, trong đó mỗi tủ thuộc về một người và có trọng lượng. Chúng tôi được phép cài đặt chính xác $m$ kết nối không định hướng giữa các tủ khóa riêng biệt. Mỗi kết nối giữa tủ khóa $u$ và $v$ tốn $au + av$."
date: "2026-06-15T23:12:05+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1255
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 601 (Div. 2)"
rating: 1100
weight: 1255
solve_time_s: 827
verified: false
draft: false
---

[CF 1255B - Tủ khóa tủ lạnh](https://codeforces.com/problemset/problem/1255/B) 

**Đánh giá:** 1100 
**Tags:** đồ thị, thực hiện 
**Thời gian giải:** 13 phút 47 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống tủ khóa hoàn chỉnh, trong đó mỗi tủ thuộc về một người và có trọng lượng. Chúng tôi được phép cài đặt chính xác$m$kết nối vô hướng giữa các tủ riêng biệt. Mỗi kết nối giữa tủ khóa$u$Và$v$chi phí$a_u + a_v$. 

Ràng buộc về cấu trúc là khó khăn chính: một tủ khóa được coi là không an toàn trừ khi chỉ chủ sở hữu của nó mới có thể truy cập nó theo quy tắc rằng quyền sở hữu gắn liền với kiến ​​thức về tất cả các khía cạnh sự cố. Nếu một tủ khóa được kết nối theo cách cho phép bất kỳ người nào khác suy ra tất cả thông tin mở khóa được yêu cầu một cách độc lập thì tủ khóa đó sẽ có thể truy cập được đối với người khác và không còn hợp lệ. 

Điều kiện đơn giản hóa thành một thuộc tính đồ thị: mỗi đỉnh phải có ít nhất một cạnh sự cố, nếu không thì mọi người đều có thể truy cập được nó. Quan trọng hơn, bất kỳ cấu hình nào cho phép một đỉnh bị cô lập hoặc được “che phủ” không đầy đủ đều vi phạm quyền riêng tư. Về cơ bản, vấn đề buộc chúng ta phải đảm bảo một cấu trúc trong đó mọi nút đều tham gia vào ít nhất một cạnh, trong khi chúng ta có thể tự do lặp lại các cạnh. 

Chúng ta phải xây dựng chính xác$m$cạnh giảm thiểu tổng chi phí. 

Những hạn chế$n \le 1000$,$m \le n$, gợi ý rằng$O(n^2)$các công trình xây dựng đều ổn, nhưng bất cứ điều gì theo cấp số nhân hoặc liên quan đến tìm kiếm tổ hợp nặng nề thì không. 

Trường hợp cạnh tinh tế xuất hiện khi$m < n-1$. Nếu chúng ta cố gắng “kết nối mọi thứ ở mức tối thiểu” thì cây sẽ sử dụng$n-1$các cạnh, do đó chúng ta thậm chí không thể tạo thành một cấu trúc được kết nối. Tuy nhiên, vấn đề không yêu cầu kết nối, chỉ có hiệu lực của các điều kiện riêng tư. Đây là nơi mà nhiều công trình tham lam không chính xác thất bại: họ ngầm cho rằng cây bao trùm là cần thiết. 

Một trường hợp cạnh khác là khi tất cả các trọng số đều lớn hoặc bằng 0. Vì chi phí chỉ phụ thuộc vào tổng cặp nên việc sử dụng nhiều lần đỉnh có trọng số nhỏ nhất là rất quan trọng; chiến lược kết nối thống nhất ngây thơ không thể giảm thiểu chi phí. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các tập hợp có thể có của$m$cạnh giữa$\binom{n}{2}$cặp, thực thi các ràng buộc về tính hợp lệ và chi phí tính toán. Ngay cả khi hạn chế ở các biểu đồ đơn giản, đây là$\binom{n^2}{m}$-scale, điều đó hoàn toàn không thể thực hiện được. 

Sự đơn giản hóa quan trọng đến từ cấu trúc chi phí: mọi chi phí ở biên đều là phụ phí ở các điểm cuối của nó. Điều này gợi ý rằng các giải pháp tối ưu sẽ sử dụng lại rất nhiều đỉnh có trọng số nhỏ nhất, bởi vì việc ghép một trọng số lớn với một trọng số lớn khác luôn tệ hơn so với việc ghép nó với trọng số tối thiểu. 

Cái nhìn sâu sắc về cấu trúc thứ hai là vì các cạnh có thể lặp lại nên chúng ta không xây dựng một cấu trúc tổ hợp như cây hoặc kết hợp. Chúng tôi đang phân phối$m$các cạnh độc lập, mỗi cạnh được chọn tối ưu dựa trên trạng thái hiện tại. 

Khi chúng ta sửa một đỉnh$p$với trọng lượng tối thiểu, mọi cạnh tối ưu sẽ liên quan đến$p$, ngoại trừ một vài cạnh ban đầu cần thiết để đáp ứng tính khả thi khi$m$lớn so với các ràng buộc. Điều này giúp giảm bớt vấn đề trong việc quyết định cách kết nối mọi thứ thông qua một trung tâm. 

Một cấu trúc hợp lệ trước tiên phải đảm bảo mọi đỉnh đều được “chạm” ít nhất một lần bằng cách nối nó với đỉnh có trọng lượng tối thiểu, tạo thành một ngôi sao. Điều này sử dụng$n-1$các cạnh. Nếu như$m > n-1$, các cạnh còn lại nên sử dụng lại cặp rẻ nhất$(p, q)$, Ở đâu$q$là đỉnh nhỏ thứ hai vì điều đó giảm thiểu chi phí lặp lại. 

Nếu như$m < n-1$, chúng ta thậm chí không thể đảm bảo rằng tất cả các đỉnh đều liên quan theo cách thỏa mãn các ràng buộc về giá trị, do đó không có nghiệm nào tồn tại. 

Do đó, giải pháp được điều khiển bởi đường trục hình sao cộng với các cạnh chi phí tối thiểu lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các bộ cạnh | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng ngôi sao với việc tái sử dụng tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm chỉ số$p$của phần tử trọng lượng tối thiểu. 

Đỉnh này là trung tâm tối ưu vì bất kỳ cạnh nào liên quan đến nó đều rẻ hơn so với việc thay thế nó bằng một điểm cuối nặng hơn. 
2. Nếu$m < n - 1$, xuất ngay$-1$. 

Chúng tôi thậm chí không thể kết nối tất cả các đỉnh một lần, điều này được yêu cầu để có hiệu lực. 
3. Xây dựng một ngôi sao bằng cách kết nối$p$đến mọi đỉnh khác. 

Điều này sử dụng$n-1$các cạnh và đảm bảo mọi đỉnh đều liên quan đến ít nhất một cạnh. 
4. Nếu$m = n-1$, xuất kết cấu này. 

Ngôi sao đã tối ưu vì mọi cạnh đều sử dụng tâm rẻ nhất có thể. 
5. Nếu$m > n-1$, chọn bất kỳ đỉnh nào$q \neq p$(thường là trọng lượng nhỏ thứ hai). 

Cạnh$(p, q)$có chi phí tối thiểu có thể có trong số tất cả các cạnh lặp lại liên quan đến$p$. 
6. Nối thêm$m - (n-1)$bản sao của cạnh$(p, q)$. 

Sự lặp lại được cho phép và lựa chọn này giữ cho chi phí gia tăng ở mức tối thiểu. 

### Tại sao nó hoạt động 

Chi phí của bất kỳ cạnh nào là tuyến tính tại các điểm cuối của nó, do đó việc giảm thiểu tổng chi phí sẽ giảm thiểu mức độ thường xuyên xuất hiện các trọng số lớn ở các cạnh. Bất kỳ giải pháp khả thi nào đều phải chạm vào mọi đỉnh ít nhất một lần, điều này buộc ít nhất$n-1$điểm cuối cạnh được phân phối. Cách rẻ nhất để phân phối các điểm cuối này là tập trung tất cả các kết nối ở đỉnh có trọng số tối thiểu. Sau thời điểm đó, bất kỳ cạnh bổ sung nào đều độc lập với các yêu cầu kết nối, do đó, mỗi cạnh lại phải giảm thiểu tổng điểm cuối, điều này đạt được bằng cách sử dụng nhiều lần cặp nhỏ nhất hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        p = min(range(n), key=lambda i: a[i])

        if m < n - 1:
            print(-1)
            continue

        edges = []

        for i in range(n):
            if i != p:
                edges.append((p + 1, i + 1))

        if m == n - 1:
            cost = sum(a[u - 1] + a[v - 1] for u, v in edges)
            print(cost)
            for u, v in edges:
                print(u, v)
            continue

        # m > n - 1
        q = 0 if p != 0 else 1
        cost = sum(a[u - 1] + a[v - 1] for u, v in edges)

        extra
```
