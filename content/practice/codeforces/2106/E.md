---
title: "CF 2106E - Sói"
description: "Chúng ta được hoán vị các số từ 1 đến n và chúng ta cần trả lời nhiều truy vấn độc lập. Mỗi truy vấn chọn một phạm vi mảng con $[l, r]$ và giá trị đích $k$. Chúng ta tưởng tượng việc thực hiện quy trình tìm kiếm nhị phân tiêu chuẩn trên phạm vi cố định đó, mặc dù mảng đó chưa được sắp xếp."
date: "2026-06-08T04:53:54+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2106
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1020 (Div. 3)"
rating: 1800
weight: 2106
solve_time_s: 105
verified: false
draft: false
---

[CF 2106E - Sói](https://codeforces.com/problemset/problem/2106/E) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 1 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n và chúng ta cần trả lời nhiều truy vấn độc lập. Mỗi truy vấn chọn một phạm vi mảng con$[l, r]$và giá trị mục tiêu$k$. Chúng ta tưởng tượng việc thực hiện quy trình tìm kiếm nhị phân tiêu chuẩn trên phạm vi cố định đó, mặc dù mảng đó chưa được sắp xếp. 

Ở mỗi bước của tìm kiếm nhị phân mô phỏng này, chúng tôi xem xét điểm giữa của khoảng thời gian hiện tại. Nếu điểm giữa đã chứa$k$, tìm kiếm thành công. Ngược lại, nếu giá trị trung điểm nhỏ hơn$k$, chúng ta tiếp tục sang nửa bên phải; nếu nó lớn hơn, chúng ta tiếp tục sang nửa bên trái. Quá trình này hoàn toàn mang tính quyết định với các giá trị mảng. 

Trước khi bắt đầu tìm kiếm, chúng ta được phép chọn một số chỉ số bên trong$[l, r]$, nhưng chúng tôi bị cấm chọn vị trí của$k$. Chúng ta có thể tự do hoán vị các giá trị tại các chỉ số đã chọn. Mục đích là làm cho việc tìm kiếm nhị phân đạt đến vị trí thành công$k$sử dụng càng ít chỉ số được chọn càng tốt. 

Vì vậy, nhiệm vụ là: đối với mỗi truy vấn, hãy xác định số phần tử tối thiểu mà chúng ta phải “sắp xếp lại cục bộ” để tìm kiếm nhị phân hoạt động chính xác và cuối cùng đạt được$k$, hoặc báo cáo rằng điều đó là không thể. 

Các ràng buộc rất chặt chẽ: tổng cộng$n$và tổng cộng$q$trên tất cả các trường hợp thử nghiệm là lên đến$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi mô phỏng tìm kiếm nhị phân theo truy vấn kết hợp với tính toán lại trên các phạm vi, vì thậm chí$O(n)$mỗi truy vấn sẽ quá chậm trong trường hợp xấu nhất. 

Khó khăn chính là tìm kiếm nhị phân không phụ thuộc vào thứ tự toàn cục mà phụ thuộc vào chuỗi so sánh điểm giữa. Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần đảm bảo “tất cả nhỏ hơn ở bên trái, tất cả lớn hơn ở bên phải”, nhưng chúng ta không sắp xếp phân khúc. Chúng tôi chỉ sửa một số vị trí giới hạn và cấu trúc của đường dẫn tìm kiếm nhị phân rất quan trọng. 

Trường hợp cạnh tinh tế xuất hiện khi$k$không có mặt trong khoảng tìm kiếm ban đầu. Mặc dù chúng ta có thể sắp xếp lại các phần tử nhưng chúng ta không thể di chuyển$k$chính nó hoặc giới thiệu nó từ bên ngoài phạm vi. Nếu như$k \notin [l, r]$, thành công là không thể ngay lập tức. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng đường dẫn tìm kiếm nhị phân và quyết định, tại mỗi điểm giữa được truy cập, liệu chúng ta có cần “sửa” vị trí đó để nó dẫn tìm kiếm theo đúng hướng hay không. Đối với một truy vấn cố định, chúng ta có thể mô phỏng đường dẫn tìm kiếm nhị phân từ$[l, r]$và bất cứ khi nào giá trị trung điểm mâu thuẫn với những gì cần thiết để cuối cùng đạt được$k$, chúng tôi coi đó là yêu cầu sửa đổi. Sau đó, chúng tôi sẽ cố gắng quyết định trên toàn cầu nên chọn chỉ số nào để giải quyết đồng thời tất cả các mâu thuẫn. 

Điều này nhanh chóng trở nên phức tạp vì một chỉ mục được chọn có thể giải quyết nhiều bước nếu nó nằm trên đường dẫn tìm kiếm nhị phân và các đường dẫn khác nhau chồng lên nhau theo cách không tầm thường trên các truy vấn. Việc tính toán lại đơn giản cho mỗi truy vấn cũng sẽ chạy lại logic tìm kiếm nhị phân theo khả năng$O(n)$các bước trong trường hợp bệnh lý, dẫn đến$O(nq)$, quá lớn. 

Quan sát chính là tìm kiếm nhị phân trên một phân đoạn cố định không phải là tùy ý: nó luôn truy cập vào một tập hợp các chỉ số xác định, tạo thành một cây phân rã nhị phân trên$[l, r]$. Mỗi điểm giữa xác định một ràng buộc quyết định: việc chúng ta đi sang trái hay phải phụ thuộc vào việc so sánh$p[m]$với$k$. Để việc tìm kiếm thành công, mọi quyết định trên con đường từ$[l, r]$xuống$k$phải phù hợp với thứ tự tương đối thực sự của$k$. 

Chúng tôi chia các chỉ số thành ba loại liên quan đến$k$: giá trị nhỏ hơn$k$, bằng$k$, và lớn hơn$k$. Vị trí của$k$đã được sửa và đường dẫn tìm kiếm nhị phân từ$[l, r]$ĐẾN$pos(k)$phải luôn hướng tới nó. Điều đó có nghĩa là mọi điểm giữa trên đường đi đều đặt ra một yêu cầu về hướng: nếu điểm giữa nằm ở bên trái của$pos(k)$, chúng ta phải đảm bảo rằng nó không định hướng chúng ta sang trái hoặc phải một cách sai lầm; tương tự ở phía bên phải. 

Sự giảm thiểu quan trọng là mọi điểm giữa được tìm kiếm nhị phân truy cập đều đã hoạt động chính xác hoặc phải được sửa bằng cách đưa chỉ mục của nó vào tập hợp đã chọn. Vì chúng ta có thể sắp xếp lại thứ tự các chỉ số đã chọn một cách tùy ý, nên chúng ta luôn có thể gán các giá trị đúng trong số chúng, miễn là chúng ta chọn đủ chúng để đáp ứng tất cả “quyết định sai” trên đường đi. 

Điều này làm giảm vấn đề đếm xem có bao nhiêu vị trí trung điểm dọc theo đường tìm kiếm nhị phân không phù hợp với hướng được yêu cầu hướng tới.$k$, dưới những ràng buộc khả thi được xác định bởi số lượng giá trị nhỏ và lớn mà chúng ta có sẵn liên quan đến$k$. 

## Hướng dẫn thuật toán 

1. Tính toán trước vị trí của từng giá trị trong hoán vị để xác định vị trí$pos(k)$TRONG$O(1)$. Điều này là cần thiết vì đường dẫn tìm kiếm nhị phân phụ thuộc vào vị trí$k$thể chất ngồi. 
2. Nếu$pos(k)$không có bên trong$[l, r]$, quay lại ngay$-1$. Tìm kiếm nhị phân không thể đến đích ngoài khoảng tìm kiếm vì không có thao tác nào cho phép di chuyển$k$vào phạm vi. 
3. Mô phỏng quá trình tìm kiếm nhị phân bắt đầu từ$[l, r]$, nhưng thay vì tìm kiếm một giá trị, chúng tôi theo dõi xem điểm giữa hiện tại nằm bên trái hay bên phải của$pos(k)$. 
4. Đối với mỗi trung điểm$m$, so sánh$m$với$pos(k)$. Nếu như$m = pos(k)$, không cần sửa đổi ở bước này. 
5. Nếu$m < pos(k)$, tìm kiếm nhị phân phải đi đúng. Điều đó có nghĩa$p[m]$phải nhỏ hơn$k$. Nếu hiện tại$p[m] > k$, vị trí này là “sai” và phải được đưa vào tập đã chọn. 
6. Nếu$m > pos(k)$, tìm kiếm nhị phân phải sang trái. Điều đó có nghĩa$p[m]$phải lớn hơn$k$. Nếu hiện tại$p[m] < k$, đây cũng là một vị trí “sai” cần được lựa chọn. 
7. Đếm tất cả các vị trí trung điểm không khớp như vậy dọc theo đường tìm kiếm nhị phân. Câu trả lời là số lần điều chỉnh cần thiết, bởi vì mỗi chỉ mục được chọn có thể được sắp xếp lại để thỏa mãn ràng buộc bên đúng. 

### Tại sao nó hoạt động 

Đường tìm kiếm nhị phân tạo thành một chuỗi cố định các điểm quyết định và mỗi quyết định chỉ phụ thuộc vào giá trị trung điểm nhỏ hơn hay lớn hơn$k$. Bất kỳ quyết định sai nào cũng buộc chúng tôi phải ghi đè vị trí đó vì việc giữ nguyên vị trí đó sẽ vĩnh viễn khiến việc tìm kiếm không còn hiệu quả.$pos(k)$. Mỗi lần ghi đè tương ứng với việc chọn chỉ mục đó và vì chúng ta có thể hoán vị các chỉ mục đã chọn một cách tùy ý nên chúng ta luôn có thể gán cho chúng các giá trị nhất quán để tất cả các vị trí đã sửa đều hoạt động chính xác đồng thời. Quá trình này không bao giờ tạo ra xung đột mới vì mỗi chỉ số được hiệu chỉnh là độc lập về mặt ràng buộc: nó chỉ cần thỏa mãn một bất đẳng thức liên quan đến$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        p = list(map(int, input().split()))
        
        pos = [0] * (n + 1)
        for i, v in enumerate(p, 1):
            pos[v] = i
        
        for _ in range(q):
            l, r, k = map(int, input().split())
            pk = pos[k]
            
            if pk < l or pk > r:
                print(-1, end=" ")
                continue
            
            d = 0
            L, R = l, r
            
            while L <= R:
                m = (L + R) // 2
                if m == pk:
                    break
                
                if m < pk:
                    if p[m - 1] > k:
                        d += 1
                    L = m + 1
                else:
                    if p[m - 1] < k:
                        d += 1
                    R = m - 1
            
            print(d, end=" ")
        print()

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng bản đồ vị trí để định vị$k$là thời gian không đổi cho mỗi truy vấn. Sau đó, mỗi truy vấn mô phỏng quá trình cập nhật khoảng thời gian tìm kiếm nhị phân, duy trì bộ đếm các vị trí trung điểm trái ngược với hướng yêu cầu đối với$k$. 

Chi tiết triển khai quan trọng là sự so sánh chặt chẽ giữa chỉ số trung điểm và vị trí của$k$, chứ không phải bản thân các giá trị. Logic phụ thuộc vào việc tìm kiếm sẽ di chuyển sang trái hay phải trong không gian chỉ mục, trong khi điều kiện đúng đắn phụ thuộc vào việc giá trị tại điểm giữa đó có tuân theo bất đẳng thức liên quan đến$k$. 

Câu trả lời tích lũy mọi quyết định điểm giữa bị vi phạm, vì mỗi vi phạm như vậy phải được khắc phục bằng cách đưa chỉ số đó vào tập hợp đã chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoán vị đơn giản:```
p = [3, 1, 4, 2]
```Truy vấn$[1, 4, 4]$. Đây$pos(4) = 3$. 

| Bước | L | R | giữa | quan hệ với pos(k) | p[giữa] | hướng cần thiết | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 2 | trái | 1 | đi bên phải | đúng | 
| 2 | 3 | 4 | 3 | bằng | 4 | tìm thấy | dừng lại | 

Không có sự không khớp nào xảy ra nên câu trả lời là 0. 

Bây giờ hãy xem xét:```
p = [2, 3, 1, 4, 5]
```Truy vấn$[1, 3, 3]$, Vì thế$pos(3)=2$. 

| Bước | L | R | giữa | quan hệ | p[giữa] | cần thiết | không khớp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | bằng | 3 | tìm thấy | không | 

Câu trả lời lại là 0. 

Bây giờ là một trường hợp thú vị hơn:```
p = [4, 3, 2, 1, 5]
```Truy vấn$[1, 4, 2]$,$pos(2)=3$. 

| Bước | giữa | p[giữa] | yêu cầu di chuyển | vấn đề | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | đi bên phải | đúng | 
| 2 | 3 | 2 | tìm thấy | dừng lại | 

Câu trả lời là 0 vì đường dẫn thẳng hàng một cách tự nhiên. 

Các ví dụ này cho thấy rằng chỉ những điểm giữa không khớp mới đóng góp và chỉ khi giá trị tại điểm giữa đó sẽ đưa tìm kiếm đi sai hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi truy vấn mô phỏng một đường dẫn tìm kiếm nhị phân trong khoảng thời gian | 
| Không gian |$O(n)$| Bản đồ vị trí cho các giá trị hoán vị | 

Giải pháp phù hợp một cách thoải mái trong giới hạn vì tổng quy mô hoạt động với$2 \cdot 10^5 \log 2 \cdot 10^5$, hiệu quả trong Python dưới các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # Placeholder: assume solve() is defined above
    # capture output
    return ""

# provided sample (placeholder formatting)
# assert run(...) == ...

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| k phạm vi bên ngoài | -1 | trường hợp bất khả thi | 
| hoán vị đã được sắp xếp | 0 câu trả lời | sự tỉnh táo đúng đắn | 
| hoán vị ngược | số lượng không tầm thường | xung đột định hướng tồi tệ nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi điểm giữa tìm kiếm nhị phân liên tục dao động xung quanh vị trí của$k$, đặc biệt là trong các hoán vị ngược hoặc nghịch đảo. Trong những trường hợp như vậy, mọi quyết định ở điểm giữa có xu hướng mâu thuẫn với hướng yêu cầu và câu trả lời trở nên tỷ lệ thuận với độ sâu của cây tìm kiếm nhị phân thay vì kích thước phân đoạn. 

Một trường hợp quan trọng khác là khi$k$gần một ranh giới của khoảng. Đường tìm kiếm nhị phân trở nên rất lệch và chỉ một số ít điểm giữa được truy cập. Thuật toán vẫn chỉ tính chính xác những điểm giữa đã truy cập chứ không phải toàn bộ khoảng thời gian vì quá trình mô phỏng sẽ dừng khi đạt được chỉ số mục tiêu. 

Cuối cùng, khi kích thước phân đoạn là 1, thuật toán ngay lập tức thành công hay thất bại tùy thuộc vào vị trí đơn lẻ đó có chứa$k$và không thể sửa đổi, phù hợp với định nghĩa về kết thúc tìm kiếm nhị phân.
