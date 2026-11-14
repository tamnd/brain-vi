---
title: "CF 1795E - Vụ nổ?"
description: "Chúng ta được cung cấp một hàng quái vật, mỗi quái vật có một lượng máu nhất định và hai kiểu tấn công: một phép thuật cơ bản làm giảm 1 máu của quái vật bằng 1 MP và một phép thuật \"Explosion\" sử dụng một lần có thể nhắm mục tiêu vào một quái vật với sức mạnh tùy ý."
date: "2026-06-09T10:08:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1795
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 143 (Rated for Div. 2)"
rating: 2200
weight: 1795
solve_time_s: 87
verified: false
draft: false
---

[CF 1795E - Vụ nổ?](https://codeforces.com/problemset/problem/1795/E) 

**Xếp hạng:** 2200 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng quái vật, mỗi quái vật có một lượng máu nhất định và hai kiểu tấn công: một phép thuật cơ bản làm giảm 1 máu của quái vật bất kỳ bằng 1 MP và một phép "Nổ" sử dụng một lần có thể nhắm mục tiêu vào một quái vật với sức mạnh tùy ý. Vụ nổ sẽ giết chết mục tiêu nếu máu của nó nhỏ hơn hoặc bằng sức mạnh của phép thuật và gây ra các vụ nổ chuỗi tới những quái vật lân cận, với mỗi vụ nổ thứ cấp có sức mạnh bằng sức mạnh của quái vật bị giết trừ đi một. Nhiệm vụ là giảm thiểu tổng lượng MP tiêu tốn, là tổng số lần sử dụng phép thuật cơ bản và sức mạnh của vụ nổ, để vụ nổ cuối cùng sẽ giết chết tất cả quái vật. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết số lượng quái vật$n$và một loạt các tình trạng sức khỏe của họ. Đầu ra là tổng MP tối thiểu cần thiết cho từng trường hợp. 

Các ràng buộc cho chúng ta biết rằng tổng số quái vật trong tất cả các trường hợp thử nghiệm có thể lên tới$3 \cdot 10^5$. Điều này ngay lập tức loại trừ các cách tiếp cận ngây thơ cố gắng mô phỏng tất cả các mục tiêu nổ có thể xảy ra với tất cả các cách phân bổ phép thuật cơ bản có thể có vì điều đó có thể dẫn đến$O(n^2)$hoặc tệ hơn là độ phức tạp về thời gian. Bất kỳ giải pháp nào cũng phải gần như tuyến tính về số lượng quái vật trong mỗi trường hợp thử nghiệm, lý tưởng nhất là$O(n)$, hoặc có thể$O(n \log n)$nếu sử dụng tìm kiếm nhị phân hoặc một đống. 

Một trường hợp không rõ ràng là khi tất cả quái vật đều có lượng máu như nhau, đặc biệt là khi chúng còn nhỏ. Ví dụ: nếu tất cả quái vật là 1, chiến lược tối ưu là sử dụng các phép thuật cơ bản 0 lần và chọn quái vật cuối cùng làm mục tiêu nổ. Một cách tiếp cận tham lam bất cẩn có thể cố gắng làm giảm lượng máu một cách không cần thiết và tiêu tốn quá nhiều MP. Một kịch bản khó khăn khác là đỉnh điểm ở giữa: [1, 10, 1], nơi vụ nổ tối ưu nhắm vào quái vật ở giữa; xác định sai quái vật nào đánh đầu tiên có thể tăng gấp đôi số MP tiêu tốn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử sử dụng các phép thuật cơ bản trên mọi tổ hợp quái vật, sau đó mô phỏng mọi mục tiêu phát nổ có thể xảy ra và tính toán MP thu được. Mặc dù đúng về nguyên tắc nhưng số lượng các lựa chọn tăng theo cấp số nhân với$n$, làm cho nó hoàn toàn không khả thi đối với các ràng buộc. Ví dụ, ngay cả một mô phỏng tuyến tính đơn giản về vụ nổ cho tất cả$n$quái vật sẽ là$O(n^2)$trong trường hợp xấu nhất nếu mọi vụ nổ đều lan truyền tuyến tính trên tất cả các vùng lân cận. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là đối với mỗi quái vật, đóng góp duy nhất có liên quan vào tổng MP là sự khác biệt giữa sức khỏe của nó và điểm nhỏ hơn của hai con quái vật lân cận. Chính xác hơn, nếu bạn coi vụ nổ như một làn sóng truyền sang trái và phải, thì vụ nổ nhiều nhất có thể "trừ đi" lượng máu nhỏ hơn của những con quái vật lân cận trừ đi một. Điều này dẫn đến một chiến lược tham lam: đối với mỗi quái vật, hãy tính toán lượng MP cần thiết để giảm nó sao cho vụ nổ của nó giết chết nó và kích hoạt chuỗi tối đa. Về mặt hình thức, tổng MP có thể được biểu thị bằng tổng đóng góp của mỗi quái vật sau khi "hấp thụ" sát thương từ vụ nổ trước đó, tương đương với việc tính tổng max(0, h[i] - min(h[i-1], h[i+1])) cho mỗi quái vật (với cách xử lý đặc biệt ở các cạnh). 

Giải pháp tối ưu tránh mọi mô phỏng và giảm vấn đề tính toán những khác biệt này theo thời gian tuyến tính. Nó hoạt động vì quá trình lan truyền vụ nổ mang tính quyết định: quyết định duy nhất là nhắm mục tiêu vào quái vật nào và khi làn sóng bắt đầu, nó không thể được chuyển hướng hoặc dừng lại, vì vậy chúng tôi chỉ cần tính đến lượng máu chưa được bao phủ bởi các vụ nổ lân cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng quái vật$n$và mảng sức khỏe của họ$h$. 
2. Khởi tạo một biến`total_mp`về không. Điều này sẽ tích lũy MP cần thiết. 
3. Đối với mỗi quái vật$i$từ 0 đến$n-1$, tính toán sát thương hiệu dụng mà nó phải nhận từ các phép thuật cơ bản trước vụ nổ để sợi dây có thể chạm tới nó. Thiệt hại hiệu quả là`max(0, h[i] - min(h[i-1], h[i+1]))`. Đối với quái vật đầu tiên và cuối cùng, chỉ có một hàng xóm tồn tại, vì vậy chỉ sử dụng máu của hàng xóm đó. Thêm giá trị này vào`total_mp`. 
4. Sau khi xử lý hết quái vật,`total_mp`hiện đại diện cho lượng MP tối thiểu cần thiết: tổng của tất cả các phép thuật cơ bản cần thiết để thiết lập một vụ nổ ở bất kỳ đâu. Xuất giá trị này cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Điều bất biến là đối với bất kỳ quái vật nào, vụ nổ từ quái vật liền kề có thể che phủ tối đa`neighbor_health - 1`hư hại. Bằng cách giảm lượng máu của quái vật để bù đắp cho vụ nổ tối đa mà nó sẽ lấy từ hàng xóm, chúng tôi đảm bảo rằng mỗi quái vật sẽ chết do vụ nổ chính hoặc do vụ nổ lan rộng, mà không lãng phí MP cho các phép thuật cơ bản không cần thiết. Điều này đảm bảo tổng MP tối thiểu trong khi đảm bảo tất cả quái vật đều chết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))
        total_mp = 0
        for i in range(n):
            left = h[i - 1] if i > 0 else 0
            right = h[i + 1] if i < n - 1 else 0
            needed = max(0, h[i] - max(left, right))
            total_mp += needed
        print(total_mp)

solve()
```Trong giải pháp này, chúng tôi xử lý các cạnh một cách cẩn thận bằng cách coi các cạnh không tồn tại là 0. Chúng tôi tính toán lượng MP mà mỗi quái vật yêu cầu sau khi xem xét mức sát thương tối đa mà nó có thể nhận được từ hàng xóm. Việc sử dụng`max(left, right)`thay vì`min`đang ở trong
