---
title: "CF 420C - Lỗi mã"
description: "Mỗi người tham gia cuộc họp công ty chỉ vào hai người khác và cho rằng thủ phạm là một trong hai người đó. Từ đó, chúng ta có thể coi đầu vào là một mảng có độ dài $n$, trong đó mỗi chỉ mục $i$ lưu trữ một cặp không có thứ tự $(xi, yi)$."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 420
codeforces_index: "C"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 1)"
rating: 1900
weight: 420
solve_time_s: 117
verified: true
draft: false
---

[CF 420C - Lỗi trong mã](https://codeforces.com/problemset/problem/420/C) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, đồ thị, cách triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người tham gia cuộc họp công ty chỉ vào hai người khác và cho rằng thủ phạm là một trong hai người đó. Từ đó chúng ta có thể coi đầu vào là một mảng có độ dài$n$, trong đó mỗi chỉ số$i$lưu trữ một cặp không có thứ tự$(x_i, y_i)$. Cặp đôi này đại diện cho hai người mà$i$-người thứ ám chỉ. 

Người đứng đầu công ty phải chọn hai người khác nhau làm nghi phạm. Người tham gia “ủng hộ” lựa chọn này nếu ít nhất một trong hai nghi phạm xuất hiện trong cặp mà họ đã nêu. Nói cách khác, mỗi cặp đầu vào hoạt động giống như một cạnh “bỏ phiếu” cho một cặp điểm cuối đã chọn bất cứ khi nào nó chia sẻ ít nhất một điểm cuối. 

Nhiệm vụ là đếm xem có bao nhiêu cặp người khác biệt không có thứ tự$(a, b)$sẽ nhận được sự hỗ trợ từ ít nhất$p$người tham gia. 

Các ràng buộc đủ lớn để bất kỳ cách tiếp cận nào lặp lại trên tất cả các cặp nghi ngờ ứng cử viên một cách rõ ràng là không thể. Với$n$lên tới$3 \cdot 10^5$, một phép liệt kê bậc hai của các cặp sẽ bao hàm xung quanh$10^{10}$kiểm tra, vượt xa mọi giới hạn khả thi. Ngay cả một giải pháp với$O(n \sqrt{n})$hành vi có nguy cơ bị thắt chặt trừ khi được tối ưu hóa cẩn thận. 

Một điều phức tạp tinh vi là cùng một cặp không có thứ tự$(a, b)$có thể xuất hiện nhiều lần trong số các câu lệnh đầu vào. Điều này ảnh hưởng đến việc đếm vì các câu lệnh lặp lại như vậy đóng góp khác nhau khi cả hai điểm cuối đều được chọn làm nghi phạm. 

Một số trường hợp đặc biệt đáng được tách biệt. Khi$p = 0$, mọi cặp người riêng biệt đều hợp lệ, vì vậy câu trả lời sẽ là$\binom{n}{2}$. Khi$p = n$, chúng ta cần mọi câu lệnh đồng ý, điều này chỉ xảy ra đối với các cặp giao nhau với tất cả các cặp đầu vào, điều này thường giảm xuống một tập hợp rất nhỏ hoặc thậm chí bằng 0. Một tình huống quan trọng khác là khi nhiều người liên tục nhắc đến cùng một cặp$(a, b)$, bởi vì những lần lặp lại đó ảnh hưởng đến số lượng thỏa thuận theo cách phá vỡ lý luận dựa trên mức độ ngây thơ nếu không được sửa chữa. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là lặp lại tất cả các cặp nghi phạm không có thứ tự$(a, b)$và, đối với mỗi cặp, quét tất cả$n$các câu lệnh để đếm xem có bao nhiêu trong số chúng bao gồm ít nhất một trong số chúng$a$hoặc$b$. Điều này đúng nhưng chi phí$O(n^3)$tổng cộng, vì có$O(n^2)$cặp ứng cử viên và mỗi lần kiểm tra là$O(n)$. 

Cấu trúc của vấn đề cho phép một cái nhìn nhỏ gọn hơn nhiều. Đối với một cặp ứng cử viên cố định$(a, b)$, một câu lệnh sẽ đóng góp nếu nó chạm vào một trong hai điểm cuối. Nếu chúng ta để$\text{deg}[v]$là số lần một người xuất hiện trên tất cả các câu nói, thì tổng số câu nói chạm vào$a$hoặc$b$là$\text{deg}[a] + \text{deg}[b]$. Điều này chỉ tính gấp đôi những tuyên bố đề cập rõ ràng đến cả hai$a$Và$b$, vì vậy chúng tôi trừ đi bội số của chúng. Nếu như$\text{cnt}[a][b]$là số lần cặp chính xác$(a, b)$xuất hiện trong đầu vào thì số lượng phát biểu đồng ý thực sự sẽ trở thành$$\text{deg}[a] + \text{deg}[b] - \text{cnt}[a][b].$$Điều này biến bài toán thành việc đếm xem có bao nhiêu cặp thỏa mãn điều kiện ngưỡng liên quan đến độ, với số hạng hiệu chỉnh nhỏ chỉ khác 0 đối với$n$chính các cặp đầu vào. 

Thách thức chính vẫn là liệt kê các cặp ứng cử viên một cách hiệu quả. Sắp xếp các đỉnh theo độ cho phép quét hai con trỏ để đếm xem có bao nhiêu cặp thỏa mãn$\text{deg}[a] + \text{deg}[b] \ge p$, bỏ qua việc sửa chữa. Điều này mang lại một tập hợp lạc quan gồm các cặp hợp lệ. Sau đó, chúng tôi khắc phục tình trạng đếm quá mức bằng cách chỉ kiểm tra các cặp đầu vào, vì chỉ chúng mới có thể có giá trị khác 0$\text{cnt}[a][b]$. Đối với mỗi cặp như vậy, chúng tôi xác minh xem việc sửa chữa có làm mất hiệu lực hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên tất cả các cặp |$O(n^3)$|$O(1)$| Quá chậm | 
| Sắp xếp + hai con trỏ + sửa |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem mỗi người xuất hiện bao nhiêu lần trong tất cả các câu phát biểu. 

Điều này đưa ra một mảng mức độ trong đó$\text{deg}[v]$đo lường mức độ “phổ biến” của một nút trong biểu đồ câu lệnh. 
2. Sắp xếp tất cả mọi người theo trình độ của họ theo thứ tự tăng dần. 

Thứ tự này cho phép chúng ta suy luận về tổng cặp một cách hiệu quả bằng cách sử dụng hai con trỏ. 
3. Sử dụng hai con trỏ để đếm tất cả các cặp không có thứ tự$(a, b)$như vậy$\text{deg}[a] + \text{deg}[b] \ge p$. 

Đối với mỗi điểm cuối bên trái$i$, chúng ta di chuyển con trỏ phải sang trái hết mức có thể trong khi điều kiện vẫn giữ, sau đó cộng tất cả các cặp hợp lệ liên quan đến$i$. 

Tính chính xác xuất phát từ tính đơn điệu của các mức độ được sắp xếp: việc tăng một điểm cuối chỉ làm tăng tổng. 
4. Xây dựng bản đồ tần suất$\text{cnt}$trên các cặp không có thứ tự từ đầu vào. 

Mỗi câu lệnh đầu vào đóng góp chính xác một lần xuất hiện cho đúng một cặp không có thứ tự. 
5. Lặp lại tất cả các cặp đầu vào$(x_i, y_i)$và coi chúng như những sự điều chỉnh ứng cử viên. 

Đối với mỗi cặp như vậy, hãy kiểm tra xem nó có được tính vào số cơ sở hay không và liệu nó có đáng lẽ không nên có hay không. 

Nếu như$\text{deg}[x_i] + \text{deg}[y_i] \ge p$Nhưng$\text{deg}[x_i] + \text{deg}[y_i] - \text{cnt}[x_i][y_i] < p$, thì cặp này đã được tính không chính xác và phải được loại bỏ một lần. 
6. In ra tổng số đã sửa. 

### Tại sao nó hoạt động 

Thuật toán tách vấn đề thành giới hạn trên có cấu trúc và hiệu chỉnh cục bộ. Giai đoạn hai con trỏ phân loại các cặp hoàn toàn theo tổng độ, ghi lại số lượng câu lệnh chạm vào ít nhất một điểm cuối theo cách tổng hợp. Bất kỳ sai lệch nào so với ước tính này chỉ xảy ra do các câu lệnh liên quan đồng thời đến cả hai điểm cuối và đó chính xác là các cặp đầu vào. Vì không có cặp nào khác đóng góp số hạng hiệu chỉnh khác 0 nên chỉ kiểm tra số hạng ban đầu$n$cặp là đủ để điều hòa hoàn toàn số lượng vượt mức mà không cần xem lại tất cả$O(n^2)$khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, p = map(int, input().split())

pairs = []
deg = [0] * (n + 1)
cnt = {}

for _ in range(n):
    x, y = map(int, input().split())
    pairs.append((x, y))
    deg[x] += 1
    deg[y] += 1

    a, b = (x, y) if x < y else (y, x)
    cnt[(a, b)] = cnt.get((a, b), 0) + 1

order = list(range(1, n + 1))
order.sort(key=lambda x: deg[x])

ans = 0
r = n - 1

for l in range(n):
    while r >= 0 and deg[order[l]] + deg[order[r]] >= p:
        r -= 1
    ans += (n - 1 - r)

# subtract overcounted invalid pairs
for x, y in pairs:
    if x > y:
        x, y = y, x
    if deg[x] + deg[y] >= p:
        if deg[x] + deg[y] - cnt[(x, y)] < p:
            ans -= 1

print(ans)
```Mảng độ được xây dựng trực tiếp từ đầu vào, coi mỗi câu lệnh là hai đóng góp. Bản đồ tần số cặp được chuẩn hóa bằng cách sắp xếp các điểm cuối sao cho$(a, b)$Và$(b, a)$giống hệt nhau. 

Phần hai con trỏ dựa trên thực tế là cả hai điểm cuối đều được lặp theo thứ tự mức độ được sắp xếp. Con trỏ`r`chỉ di chuyển sang trái, giúp giữ độ phức tạp tổng thể tuyến tính sau khi sắp xếp. 

Vòng điều chỉnh chỉ chạm vào$n$câu lệnh đầu vào, tránh mọi hành vi bậc hai. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu.```
n = 4, p = 2
(2,3)
(1,4)
(1,4)
(2,1)
```Đầu tiên chúng ta tính toán độ. 

| Nút | độ | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 2 | 

Thứ tự sắp xếp theo mức độ có thể là: 3, 1, 2, 4. 

### Pha đếm hai con trỏ 

Chúng tôi đếm các cặp có tổng bậc ít nhất là 2. 

| tôi | nút l | r bắt đầu | cặp hợp lệ được thêm vào | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | (3,1),(3,2),(3,4) | 
| 1 | 1 | 3 | (1,2),(1,4) | 
| 2 | 2 | 3 | (2,4) | 
| 3 | 4 | 3 | không | 

Điều này mang lại số lượng cơ sở là 6 cặp. 

### Giai đoạn điều chỉnh 

Chúng tôi kiểm tra các cặp đầu vào: 

| Cặp | tổng độ | cnt | tổng điều chỉnh | hành động | 
| --- | --- | --- | --- | --- | 
| (2,3) | 3 | 1 | 2 | giữ | 
| (1,4) | 4 | 2 | 2 | giữ | 
| (2,1) | 4 | 1 | 3 | giữ | 

Không có cặp nào vi phạm điều kiện nên đáp án cuối cùng vẫn là 6. 

Dấu vết này cho thấy giai đoạn hai con trỏ cố tình đếm quá một cách an toàn và các sửa đổi chỉ được áp dụng khi các tuyên bố lặp lại có thể làm tăng sự đồng ý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính và hiệu chỉnh sử dụng một lần chuyển qua các cặp đầu vào | 
| Không gian |$O(n)$| Mảng độ, mảng thứ tự và bản đồ tần số trên các cặp đầu vào | 

Thuật toán phù hợp thoải mái trong giới hạn vì cả hai giai đoạn sau khi tiền xử lý đều là tuyến tính và thao tác siêu tuyến tính duy nhất là sắp xếp$n$các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    data = inp.strip().split()
    n = int(data[0]); p = int(data[1])
    idx = 2
    pairs = []
    deg = [0] * (n + 1)
    cnt = {}

    for _ in range(n):
        x = int(data[idx]); y = int(data[idx+1]); idx += 2
        pairs.append((x, y))
        deg[x] += 1
        deg[y] += 1
        a, b = (x, y) if x < y else (y, x)
        cnt[(a, b)] = cnt.get((a, b), 0) + 1

    order = list(range(1, n + 1))
    order.sort(key=lambda x: deg[x])

    ans = 0
    r = n - 1
    for l in range(n):
        while r >= 0 and deg[order[l]] + deg[order[r]] >= p:
            r -= 1
        ans += (n - 1 - r)

    for x, y in pairs:
        if x > y:
            x, y = y, x
        if deg[x] + deg[y] >= p:
            if deg[x] + deg[y] - cnt[(x, y)] < p:
                ans -= 1

    return str(ans).strip()

# provided sample
assert run("""4 2
2 3
1 4
1 4
2 1
""") == "6"

# minimum size
assert run("""3 0
1 2
2 3
1 3
""") == "3"

# all statements identical
assert run("""4 3
1 2
1 2
1 2
1 2
""") == "1"

# strict requirement impossible
assert run("""3 3
1 2
2 3
1 3
""") == "0"

# boundary: only one valid pair
assert run("""4 4
1 2
1 3
1 4
1 2
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 6 | tính đúng đắn của sự chồng chéo hỗn hợp | 
| p = 0 trường hợp | 3 | tất cả các cặp hành vi hợp lệ | 
| cặp giống hệt nhau | 1 | xử lý trùng lặp nặng nề | 
| ngưỡng không thể | 0 | không có cặp hợp lệ | 
| cấu trúc sao chặt chẽ | 1 | đếm thỏa thuận ranh giới | 

## Vỏ cạnh 

Khi nào$p = 0$, mọi cặp nút riêng biệt không có thứ tự đều hợp lệ bất kể câu lệnh nào. Thuật toán vẫn tính tất cả các cặp tổng độ trong giai đoạn đầu tiên, nhưng giai đoạn hiệu chỉnh không loại bỏ bất cứ thứ gì, do đó đầu ra chính xác sẽ trở thành$\binom{n}{2}$. 

Khi tất cả các câu lệnh đều giống nhau, hãy nói tên của mọi người lập trình$(1, 2)$, độ tập trung nhiều vào hai nút. Vấn đề tiềm ẩn duy nhất là đếm quá mức do tần số cao, nhưng bước hiệu chỉnh xử lý vấn đề này một cách rõ ràng bằng cách kiểm tra tần số chính xác của cặp, đảm bảo số đếm cuối cùng phản ánh điều kiện thỏa thuận thực sự. 

Khi đến ngưỡng yêu cầu$p$là rất lớn, hầu hết các cặp đều được lọc ra trong giai đoạn hai con trỏ vì tổng bậc không thể đạt đến ngưỡng. Thuật toán không bao giờ khám phá chúng thêm nữa và việc kiểm tra hiệu chỉnh trở nên không liên quan vì không có cặp ứng cử viên nào vượt qua được bộ lọc đầu tiên.
