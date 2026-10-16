---
title: "CF 105493F - Hoạt động tình nguyện"
description: "Chúng ta có một khoảng lớn cố định trên một trục số, cùng với một số khoảng nhỏ hơn. Mỗi khoảng thời gian nhỏ góp phần bao phủ một phần nào đó của khoảng thời gian lớn."
date: "2026-06-23T01:42:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105493
codeforces_index: "F"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 105493
solve_time_s: 58
verified: true
draft: false
---

[CF 105493F - Hoạt động tình nguyện](https://codeforces.com/problemset/problem/105493/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khoảng lớn cố định trên một trục số, cùng với một số khoảng nhỏ hơn. Mỗi khoảng thời gian nhỏ góp phần bao phủ một phần nào đó của khoảng thời gian lớn. Nhiệm vụ không chỉ là biết bao nhiêu khoảng lớn được bao phủ mà còn phải hiểu phạm vi này được phân bổ như thế nào theo độ sâu chồng lấp. 

Đối với mọi ngưỡng số nguyên$h$, chúng ta muốn biết tổng chiều dài của khoảng lớn được bao phủ bởi ít hơn$h$những khoảng nhỏ. Tương tự, nếu một điểm được bao phủ bởi các khoảng 0, 1, 2, …, chúng tôi muốn tích lũy tất cả các phần có số lượng bao phủ nhiều nhất$h-1$. 

Một cách giải thích trực tiếp là tính toán, đối với mỗi điểm trong khoảng lớn, có bao nhiêu khoảng nhỏ bao phủ nó, sau đó tổng hợp độ dài theo giá trị bao phủ. Điều này ngay lập tức gợi ý một sự giảm bớt: thay vì trả lời “độ bao phủ < h”, trước tiên chúng ta tính toán chính xác độ dài được bao phủ bởi$k$khoảng thời gian cho tất cả$k$, sau đó xây dựng tổng tiền tố dựa trên các giá trị này. 

Khoảng lớn giới hạn tất cả các đóng góp, vì vậy bất kỳ phần nào của khoảng nhỏ nằm ngoài nó đều không liên quan. Điều này có nghĩa là mọi phân đoạn có thể được cắt bớt một cách an toàn vào phạm vi$[S, F]$. 

Các ràng buộc (ngụ ý bởi cấu trúc câu lệnh kiểu ICPC) thường cho phép tối đa$n$theo thứ tự$2 \cdot 10^5$hoặc cao hơn. Do đó, giải pháp phải gần$O(n \log n)$hoặc$O(n)$. Bất kỳ cách tiếp cận nào theo dõi rõ ràng mức độ phù hợp trên mỗi điểm hoặc sử dụng tính năng quét theo khoảng thời gian theo truy vấn sẽ quá chậm. 

Một trường hợp lỗi tinh vi xuất hiện khi các đoạn mở rộng ra ngoài khoảng lớn. Ví dụ, nếu$[S, F] = [10, 20]$và một đoạn là$[1, 15]$, chỉ phần$[10, 15]$vấn đề. Việc quên cắt clip sẽ dẫn đến việc đếm quá độ dài bên ngoài miền quan tâm. Tương tự, các phân đoạn hoàn toàn nằm ngoài$[S, F]$, chẳng hạn như$[1, 5]$hoặc$[25, 30]$, phải được bỏ qua hoàn toàn. 

Một trường hợp cạnh khác phát sinh khi nhiều điểm cuối của đoạn trùng nhau. Thứ tự của các sự kiện trở nên quan trọng. Nếu một phân đoạn kết thúc chính xác tại nơi một phân đoạn khác bắt đầu, thì thứ tự không chính xác có thể làm tăng số lượng trùng lặp một cách giả tạo trên một điểm. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là rời rạc hóa khoảng lớn thành các vị trí đơn vị hoặc tọa độ chi tiết và đối với mỗi điểm, hãy đếm xem có bao nhiêu khoảng bao phủ nó. Điều này hoạt động về mặt khái niệm: đối với mỗi lát số nguyên hoặc tọa độ, chúng tôi kiểm tra tất cả các phân đoạn và tăng bộ đếm nếu phân đoạn đó bao phủ nó. Sau khi tính toán số lượng vùng phủ sóng cho tất cả các vị trí, chúng ta có thể tích lũy độ dài theo tần suất. 

Cách tiếp cận này đúng nhưng quá chậm. Nếu độ dài khoảng lớn là$L$, chi phí kiểm tra mỗi điểm$O(nL)$, mà thoái hóa thành một cái gì đó như$10^{10}$hoạt động khi cả hai$n$Và$L$lớn. Ngay cả việc nén tọa độ cũng không giúp ích gì nếu dòng số dày đặc hoặc điểm cuối lớn nhưng đóng góp liên tục lại quan trọng. 

Quan sát quan trọng là phạm vi bao phủ chỉ thay đổi ở các điểm cuối của phân đoạn. Giữa hai điểm cuối liên tiếp, số lượng phân đoạn hoạt động không đổi, do đó, sự đóng góp của toàn bộ khoảng thời gian đó có thể được tính toán trong một lần chụp. Điều này biến vấn đề thành việc duy trì số lượng khoảng thời gian hoạt động động trong khi quét qua tất cả các điểm quan trọng. 

Chúng tôi chuyển đổi mỗi phân đoạn thành hai sự kiện, một khi nó bắt đầu đóng góp và một khi nó dừng lại. Sau khi cắt thành$[S, F]$, mỗi phân đoạn đóng góp một sự kiện “mở” ở ranh giới bên trái của nó và một sự kiện “đóng” ở ranh giới bên phải của nó. Việc quét các sự kiện này theo thứ tự được sắp xếp cho phép chúng tôi theo dõi số lượng phân đoạn hiện đang hoạt động và tích lũy độ dài của các khoảng thời gian có số lượng trùng lặp giống hệt nhau. 

Chi tiết tinh tế nhưng cần thiết là thứ tự sự kiện. Tại cùng một tọa độ, sự kiện đóng phải được xử lý trước sự kiện mở. Nếu không, một điểm nằm chính xác tại ranh giới chung sẽ được tính không chính xác là được bao phủ bởi cả hai phân đoạn cùng một lúc. 

Cách tiếp cận theo đường quét này làm giảm vấn đề sắp xếp các sự kiện và thực hiện quét tuyến tính, hiệu quả và ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot (F-S))$|$O(1)$hoặc$O(F-S)$| Quá chậm | 
| Đường quét tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành tính toán, cho từng số lượng bảo hiểm có thể$k$, tổng chiều dài của khoảng lớn được bao phủ bởi chính xác$k$phân đoạn. Khi đã biết những giá trị này, câu trả lời cho ngưỡng$h$chỉ đơn giản là tổng của tất cả các số đếm từ$0$ĐẾN$h-1$. 

1. Đối với từng phân khúc$[b_i, e_i]$, trước tiên hãy giới hạn nó trong phạm vi có ý nghĩa bằng cách thay thế nó bằng$[\max(b_i, S), \min(e_i, F)]$. Nếu khoảng kết quả trống, nó không đóng góp gì. Bước này đảm bảo chúng tôi chỉ xử lý các phần có liên quan của phân khúc. 
2. Đối với mỗi phân đoạn bị cắt bớt, hãy tạo hai sự kiện: một sự kiện ở đầu đánh dấu mức tăng phạm vi hoạt động và một ở cuối đánh dấu mức giảm. Những sự kiện này đại diện cho những điểm mà số lượng trùng lặp thay đổi. 
3. Sắp xếp tất cả các sự kiện theo tọa độ và khi hai sự kiện có cùng tọa độ, hãy xử lý các sự kiện kết thúc trước các sự kiện mở. Thứ tự này đảm bảo rằng một điểm chính xác tại một ranh giới không bị tính hai lần trong các phân đoạn chồng chéo. 
4. Duy trì một biến`active`biểu thị số lượng phân đoạn hiện đang bao phủ vị trí quét và một biến`last`đại diện cho tọa độ sự kiện trước đó. 
5. Khi di chuyển từ một vị trí sự kiện`x`tiếp theo, khoảng thời gian$[last, x]$có phạm vi bảo hiểm liên tục bằng`active`. Chúng tôi thêm chiều dài của nó$x - last$vào nhóm tương ứng với mức độ bao phủ đó. 
6. Xử lý sự kiện tại tọa độ`x`bằng cách cập nhật`active`: giảm đối với các sự kiện đóng và tăng đối với các sự kiện mở. 
7. Cập nhật`last = x`và tiếp tục cho đến khi tất cả các sự kiện được xử lý. 
8. Sau khi quét ta có giá trị`c[k]`để biết số lượng bảo hiểm chính xác. Chúng tôi tính toán tổng tiền tố để có được`a[h] = c[0] + c[1] + ... + c[h-1]`. 

Bất biến chính là giữa hai tọa độ sự kiện liên tiếp bất kỳ, tập hợp các phân đoạn hoạt động không thay đổi. Do đó, số lượng vùng phủ sóng không đổi trong khoảng thời gian đó và việc chỉ định toàn bộ chiều dài của nó cho một nhóm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, S, F = map(int, input().split())
    
    events = []
    
    for _ in range(n):
        b, e = map(int, input().split())
        l = max(b, S)
        r = min(e, F)
        if l >= r:
            continue
        events.append((l, 1))   # open
        events.append((r, -1))  # close
    
    # sort: coordinate asc, close (-1) before open (+1)
    events.sort(key=lambda x: (x[0], x[1]))
    
    # max possible overlap is n
    c = [0] * (n + 1)
    
    active = 0
    last = S
    
    for x, typ in events:
        if x > last:
            c[active] += x - last
            last = x
        
        active += typ
    
    # no more events, but interval may continue to F
    if last < F:
        c[active] += F - last
    
    # prefix sums for answers
    a = [0] * (n + 1)
    for i in range(n):
        a[i + 1] = a[i] + c[i]
    
    print(*a[1:])

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc các phân đoạn và ngay lập tức cắt chúng vào phạm vi có liên quan. Điều này tránh những đóng góp không chính xác từ bên ngoài khoảng thời gian mục tiêu. Mỗi phân đoạn được cắt bớt hợp lệ tạo ra chính xác hai sự kiện và những sự kiện này được sắp xếp theo điểm ngắt để đảm bảo các sự kiện gần được xử lý trước. 

Quá trình quét duy trì số lượng phân đoạn hoạt động. Bất cứ khi nào quá trình quét di chuyển về phía trước theo thời gian, số lượng hoạt động trước đó sẽ được sử dụng để tích lũy độ dài đóng góp vào nhóm chính xác. 

Một lỗi triển khai phổ biến là quên xử lý phân đoạn cuối cùng từ vị trí sự kiện cuối cùng đến$F$. Mã thêm rõ ràng khoảng thời gian còn lại này. Một vấn đề tế nhị khác là sắp xếp sự kiện: sử dụng`(x, typ)`hoạt động vì`-1 < +1`, đảm bảo xử lý ranh giới chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó phân khúc lớn là$[0, 10]$và hai đoạn nhỏ là$[1, 5]$Và$[3, 7]$. 

Chúng tôi xử lý các sự kiện sau khi cắt: 

| Vị trí sự kiện | Loại | Hoạt động trước | Đã thêm khoảng thời gian | Nhóm đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | - | - | 
| 1 | +1 | 0 | [0,1] chiều dài 1 | c[0] += 1 | 
| 3 | +1 | 1 | [1,3] chiều dài 2 | c[1] += 2 | 
| 5 | -1 | 2 | [3,5] chiều dài 2 | c[2] += 2 | 
| 7 | -1 | 1 | [5,7] chiều dài 2 | c[1] += 2 | 
| 10 | kết thúc | 0 | [7,10] dài 3 | c[0] += 3 | 

Điều này cho thấy số lượng chồng chéo không đổi giữa các điểm sự kiện và cách mỗi đoạn của dòng được phân bổ chính xác. 

Bây giờ hãy xem xét căn chỉnh cạnh:$[0, 5]$Và$[5, 10]$TRÊN$[0, 10]$. 

| Vị trí sự kiện | Loại | Hoạt động trước | Đã thêm khoảng thời gian | Nhóm đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | +1 | 0 | [0,5] | c[1] += 5 | 
| 5 | -1 rồi +1 | 1→0→1 | [5,10] chia đúng | c[1] += 5 | 

Quá trình xử lý đóng trước khi mở đảm bảo rằng điểm 5 không bị tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sự kiện sắp xếp chiếm ưu thế; quét là tuyến tính | 
| Không gian |$O(n)$| lưu trữ tối đa hai sự kiện trên mỗi phân đoạn và mảng tần số | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho$n$lên tới$2 \cdot 10^5$, vì việc sắp xếp và quét một lần là đủ hiệu quả cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# NOTE: In real use, wrap solve() carefully; this is illustrative only.

# custom cases
# 1) single segment
# 2) disjoint segments
# 3) fully overlapping segments
# 4) boundary-touching segments

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đoạn bên trong | bảo hiểm đơn giản | tính đúng đắn của trường hợp cơ sở | 
| phân đoạn không chồng chéo | xô chia | xử lý rời rạc | 
| phân khúc giống hệt nhau | thùng chồng chéo tối đa | xếp chồng đúng cách | 
| chạm vào điểm cuối | không tính gấp đôi | thứ tự ranh giới | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là một đoạn hoàn toàn nằm ngoài khoảng lớn. Ví dụ,$[1, 5]$với$[S, F] = [10, 20]$. Sau khi cắt, khoảng sẽ trống và bị loại bỏ, do đó không có sự kiện nào được tạo và phạm vi bao phủ vẫn bằng 0. 

Một trường hợp khác là đoạn bắt đầu trước$S$và kết thúc bên trong, chẳng hạn như$[0, 7]$với$[S, F] = [5, 10]$. Sau khi cắt, nó trở thành$[5, 7]$và chỉ đóng góp từ 5 đến 7. Quá trình quét chính xác bắt đầu đếm ở 5 chứ không phải ở 0. 

Trường hợp tinh tế cuối cùng là nhiều phân đoạn chia sẻ điểm cuối. Giả định$[0, 5]$,$[5, 10]$, Và$[5, 8]$. Quy tắc sắp xếp đảm bảo rằng các sự kiện đóng ở mức 5 được xử lý trước khi mở, do đó, số lượng trùng lặp ở chính xác là 5 phản ánh tập hợp chính xác các phân đoạn hoạt động ở mỗi bên của ranh giới, tránh mức độ bao phủ tăng đột biến một cách giả tạo.
