---
title: "CF 105911K - Xoay"
description: "Chúng ta được đưa cho một hàng tượng, ban đầu mỗi tượng chỉ về một trong bốn hướng được sắp xếp theo một chu kỳ: trước, phải, sau, trái, rồi lại quay về phía trước sau một vòng quay hoàn toàn."
date: "2026-06-21T15:28:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "K"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 42
verified: true
draft: false
---

[CF 105911K - Xoay](https://codeforces.com/problemset/problem/105911/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một hàng tượng, ban đầu mỗi tượng chỉ về một trong bốn hướng được sắp xếp theo một chu kỳ: trước, phải, sau, trái, rồi lại quay về phía trước sau một vòng quay hoàn toàn. Chúng tôi được phép thực hiện các thao tác xoay toàn bộ tất cả các bức tượng theo chiều kim đồng hồ, nhưng điểm mấu chốt là mỗi thao tác được kích hoạt bằng cách nhấn vào một bức tượng cụ thể hoặc bằng cách tự nhấn và cả hai hành động cuối cùng đều tạo ra cùng một hiệu ứng tổng thể: mọi bức tượng đều xoay 90 độ theo chiều kim đồng hồ. 

Sự khác biệt duy nhất giữa hai phép toán được phép là về mặt khái niệm chứ không phải về mặt hình học. Mỗi lần nhấn luôn xoay mỗi bức tượng đúng một bước trong chu trình. Vì hoạt động này không bao giờ ảnh hưởng đến các bức tượng riêng lẻ nên toàn bộ hệ thống chỉ phát triển thông qua việc tăng dần toàn cầu lặp đi lặp lại của tất cả các giá trị modulo 4. 

Mục tiêu là chọn một chuỗi các lần nhấn sao cho sau tất cả các lần quay, mọi bức tượng đều hướng về phía trước. Mỗi bức tượng bắt đầu bằng một giá trị trong {0, 1, 2, 3}, trong đó 0 có nghĩa là phía trước và 1, 2, 3 tương ứng với phải, sau và trái. 

Ràng buộc n có thể lớn tới 10^6, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào thử các trình tự nhấn khác nhau trên mỗi bức tượng hoặc khám phá các tổ hợp hoạt động theo cấp số nhân. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề xuống O(n) hoặc O(n log n) ở mức tệ nhất và tốt nhất là O(n). 

Một vấn đề tế nhị sẽ xuất hiện nếu một người cố gắng suy nghĩ cục bộ. Ví dụ, người ta có thể tưởng tượng việc nhấn các bức tượng khác nhau có thể tạo ra các hiệu ứng khác nhau, nhưng trên thực tế, cả hai thao tác đều tạo ra các phép biến đổi giống hệt nhau trên toàn bộ mảng. Điều này loại bỏ mọi điều khiển trên mỗi bức tượng và giảm vấn đề xuống hệ thống bù đắp toàn cầu thuần túy. 

Các trường hợp cạnh phát sinh khi tất cả các bức tượng đã được căn chỉnh, khi tất cả đều giống hệt nhau nhưng không được căn chỉnh và khi việc phân bổ các hướng tạo ra các yêu cầu xung đột. Ví dụ, đầu vào`0 1 2 3`rõ ràng là không thể sửa chữa bằng lý luận từng phần cho mỗi bức tượng, vì mỗi lần nhấn đều ảnh hưởng đến tất cả các giá trị như nhau. Đầu ra chính xác chỉ phụ thuộc vào số lượng phép quay toàn cục được yêu cầu chứ không phụ thuộc vào bất kỳ thứ tự hoạt động nào. 

## Phương pháp tiếp cận 

Ý tưởng brute-force bắt đầu bằng cách mô phỏng trình tự các lần nhấn. Vì mỗi lần nhấn sẽ xoay tất cả các bức tượng theo một bước, nên chúng ta có thể thử áp dụng k lần nhấn và kiểm tra xem liệu tất cả các giá trị có trở về 0 modulo 4 hay không. Điều này rất đơn giản: đối với mỗi k, hãy tính các giá trị được chuyển đổi và xác minh xem chúng có bằng 0 hay không. Vấn đề là về nguyên tắc k không bị giới hạn và thậm chí việc giới hạn k trong [0, 3] cho mỗi bức tượng cũng không giúp ích gì vì các hoạt động không cô lập các vị trí riêng lẻ. 

Một cách tiếp cận bạo lực có cấu trúc hơn một chút là quan sát rằng vì mọi thao tác đều giống hệt nhau nên điều duy nhất quan trọng là tổng số lần nhấn modulo 4. Chúng ta có thể thử k từ 0 đến 3, mỗi lần mô phỏng toàn bộ mảng và kiểm tra tính hợp lệ. Điều này hoạt động nhưng vẫn có giá O(4n), điều này tốt nhưng ẩn cấu trúc sâu hơn. 

Cái nhìn sâu sắc quan trọng là hệ thống chỉ có một bậc tự do: độ lệch xoay vòng toàn cầu. Nếu chúng ta thực hiện k thao tác, mỗi bức tượng sẽ tăng k modulo 4. Do đó, đối với một bức tượng bắt đầu tại ai, giá trị cuối cùng của nó trở thành (ai + k) mod 4. Để tất cả các bức tượng kết thúc ở 0, chúng ta cần (ai + k) mod 4 = 0 với mọi i. Điều này ngụ ý k ≡ -ai mod 4 với mọi i, nghĩa là tất cả ai phải đồng ý về cùng một giá trị modulo 4 sau khi dịch chuyển k. Điều này chỉ có thể thực hiện được nếu tất cả ai đều bằng modulo 4 và k được xác định duy nhất bởi giá trị đó. 

Do đó, vấn đề giảm xuống còn việc kiểm tra tính nhất quán và tính toán một độ lệch toàn cục duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của k | O(4n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Quan sát bù đắp toàn cầu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng hướng dẫn và diễn giải từng giá trị dưới dạng số nguyên trong số học modulo 4. Điều này là cần thiết vì mọi hoạt động đều bảo tồn cấu trúc mô-đun. 
2. Quan sát rằng sau khi nhấn k, mọi phần tử trở thành (ai + k) mod 4, vì vậy chúng ta đang tìm kiếm một k duy nhất làm cho tất cả các giá trị bằng 0 cùng một lúc. 
3. Chuyển đổi điều kiện (ai + k) mod 4 = 0 thành k ≡ -ai mod 4. Điều này ngụ ý rằng tất cả ai phải tạo ra cùng một giá trị k cần thiết. 
4. Tính k ứng viên từ phần tử đầu tiên là k = (4 - a0) mod 4. 
5. Xác minh tính nhất quán bằng cách kiểm tra xem mọi i, (ai + k) mod 4 có bằng 0. Nếu bất kỳ phần tử nào vi phạm điều này, thì không có giải pháp nào tồn tại theo các giả định xoay vòng toàn cầu thống nhất. 
6. Nếu nhất quán, trả về k là số lần nhấn tối thiểu vì bất kỳ chu kỳ đầy đủ bổ sung nào gồm 4 lần nhấn đều là dư thừa. 

Lý do điều này hoạt động là vì hệ thống không có sự độc lập về mặt không gian. Mọi thao tác đều áp dụng cùng một phép biến đổi cho mọi bức tượng, do đó toàn bộ không gian trạng thái thu gọn thành một biến tuần hoàn duy nhất biểu thị vòng quay toàn cầu. Thuật toán khai thác bất biến này bằng cách giảm toàn bộ mảng thành một hệ phương trình mô đun duy nhất với một ẩn số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    k = (4 - a[0]) % 4
    
    for x in a:
        if (x + k) % 4 != 0:
            print(-1)
            return
    
    print(k)

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào và ngay lập tức giảm vấn đề xuống việc tìm một độ lệch xoay toàn cục duy nhất k xuất phát từ bức tượng đầu tiên. Lựa chọn đó xác định hướng cuối cùng phải là gì: nếu bức tượng đầu tiên cần k góc quay để chạm tới phía trước, thì mọi bức tượng khác cũng phải chạm tới phía trước theo cùng k. 

Vòng lặp xác nhận giả định này. Nếu bất kỳ bức tượng nào không thẳng hàng theo cùng một k, điều đó chứng tỏ rằng không có chuỗi thao tác nào có thể cố định được cấu hình. Việc kiểm tra này là cần thiết vì nó thực thi điều kiện nhất quán toàn cục thay vì giả định điều kiện đó. 

Một chi tiết triển khai tinh tế là tính toán modulo của k. Việc sử dụng (4 - a[0]) % 4 đảm bảo rằng k nằm trong [0, 3], tránh các giá trị âm trong khi vẫn giữ được độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 1 2 3
```Chúng tôi tính toán k từ phần tử đầu tiên. 

| Bước | một [tôi] | (a[i] + k) % 4 | hợp lệ | 
| --- | --- | --- | --- | 
| tôi=0 | 0 | (0+0)%4 = 0 | vâng | 
| tôi=1 | 1 | (1+0)%4 = 1 | không | 

Vì k = 0 không thành công ngay lập tức nên chúng tôi thử k = 3 (vì (4 - 0) % 4 = 0, nhưng tính nhất quán không thành công trên toàn mảng) và chúng tôi quan sát thấy không có k đơn lẻ nào thỏa mãn tất cả các ràng buộc. Đầu ra là -1. 

Điều này cho thấy rằng các hướng hỗn hợp không thể được căn chỉnh chỉ bằng cách sử dụng các phép quay tổng thể. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
```Tính k = (4 - 2) % 4 = 2. 

| Bước | một [tôi] | (a[i] + k) % 4 | hợp lệ | 
| --- | --- | --- | --- | 
| tôi=0 | 2 | 0 | vâng | 
| tôi=1 | 2 | 0 | vâng | 
| tôi=2 | 2 | 0 | vâng | 

Tất cả các bức tượng đều thẳng hàng sau 2 lần xoay, xác nhận tính chính xác của độ lệch được tính toán. 

Điều này xác nhận rằng các mảng thống nhất luôn giảm gọn gàng theo một yêu cầu xoay vòng toàn cầu duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bức tượng được kiểm tra một lần về tính nhất quán trong một lần bù xoay được tính toán duy nhất | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng ngoài bộ nhớ đầu vào | 

Quét tuyến tính là tối ưu vì mọi phần tử đầu vào phải được đọc ít nhất một lần. Với n tối đa 10^6, điều này phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    k = (4 - a[0]) % 4
    for x in a:
        if (x + k) % 4 != 0:
            return "-1"
    return str(k)

# provided sample
assert run("4\n0 1 2 3") == "-1"

# all already front
assert run("3\n0 0 0") == "0"

# uniform rotated
assert run("3\n2 2 2") == "2"

# single element
assert run("1\n3") == "1"

# alternating impossible
assert run("4\n0 2 0 2") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 0 1 2 3 | -1 | trường hợp định hướng hỗn hợp không thể | 
| 3 0 0 0 | 0 | đã giải quyết được cấu hình | 
| 3 2 2 2 | 2 | phép quay đều khác 0 | 
| 1 3 | 1 | trường hợp cạnh phần tử đơn | 
| 4 0 2 0 2 | -1 | xen kẽ mô hình không nhất quán | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi n = 1. Đối với đầu vào`3`, thuật toán tính toán k = (4 - 3) % 4 = 1 và vòng lặp xác thực trôi qua một cách tầm thường, tạo ra đầu ra 1. Điều này đúng vì một bức tượng luôn khớp với chính nó sau một số lần quay cố định. 

Một trường hợp khác là khi tất cả các giá trị đều giống hệt nhau. Vì`2 2 2 2`, k trở thành 2 và mọi kiểm tra đều đạt vì hệ thống giảm về độ dịch chuyển đều. 

Một trường hợp thất bại là tính chẵn lẻ hỗn hợp như`0 2 0 2`. Thuật toán chọn k = 0, nhưng phần tử thứ hai vi phạm điều kiện ngay lập tức vì (2 + 0) % 4 = 2 nên từ chối cấu hình một cách chính xác.
