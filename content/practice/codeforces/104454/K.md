---
title: "CF 104454K - Danh sách việc cần làm"
description: "Chúng ta được giao một tập hợp các nhiệm vụ, mỗi nhiệm vụ có khoảng thời gian lặp lại riêng được tính bằng ngày. Nếu một nhiệm vụ có giá trị $ai$, thì khi nó được hoàn thành lần đầu vào ngày 0, nó phải được lặp lại vào các ngày $ai, 2ai, 3ai,dots$, miễn là những ngày đó nằm trong $k$ ngày tiếp theo, chúng tôi…"
date: "2026-06-30T14:28:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "K"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 84
verified: false
draft: false
---

[CF 104454K - Danh sách việc cần làm](https://codeforces.com/problemset/problem/104454/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một tập hợp các nhiệm vụ, mỗi nhiệm vụ có khoảng thời gian lặp lại riêng được tính bằng ngày. Nếu một nhiệm vụ có giá trị$a_i$, sau đó khi nó được hoàn thành lần đầu vào ngày 0, nó phải được lặp lại vào những ngày$a_i, 2a_i, 3a_i,\dots$, miễn là những ngày đó rơi vào khoảng thời gian tiếp theo$k$ngày mà chúng tôi đang lên kế hoạch. Một giá trị của$a_i = 0$là trường hợp đặc biệt khi tác vụ không tạo ra bất kỳ sự lặp lại nào trong tương lai sau lần hoàn thành đầu tiên. 

Mục tiêu là để xác định, mỗi ngày từ 1 đến$k$, cần bao nhiêu lần lặp lại nhiệm vụ vào ngày đó trên tất cả các nhiệm vụ cộng lại. 

Giải thích trực tiếp là mọi nhiệm vụ đều “sinh ra” một chuỗi ngày trong tương lai. Chúng tôi đang đếm xem có bao nhiêu chuỗi này giao nhau mỗi ngày trong cửa sổ lập kế hoạch. 

Các ràng buộc cho phép lên đến$10^5$nhiệm vụ và$10^5$ngày. Điều này loại trừ mọi giải pháp mô phỏng từng nhiệm vụ hàng ngày, vì việc truyền bá đơn giản sẽ dẫn đến$O(nk)$hành vi trong trường hợp xấu nhất, đạt đến$10^{10}$hoạt động và không khả thi. Thậm chí lặp lại trên tất cả bội số của mỗi$a_i$riêng lẻ sẽ quá chậm nếu được thực hiện mà không có cấu trúc, vì tổng số lần cập nhật phải được giới hạn cẩn thận. 

Một trường hợp khó phát hiện khi$a_i = 0$. Trong trường hợp này, việc triển khai bất cẩn có thể cố gắng xử lý nó như kích thước bước bình thường, dẫn đến chia cho 0 hoặc vòng lặp vô hạn khi tạo bội số. Một trường hợp cạnh khác là$a_i > k$, trong đó không có sự lặp lại nào xảy ra trong phạm vi quy hoạch, nhưng một vòng lặp ngây thơ vẫn có thể cố gắng xử lý những việc không cần thiết. 

Ví dụ, nếu$n=2, k=6$Và$a = [0, 1]$, thì nhiệm vụ đầu tiên không đóng góp gì sau ngày 0, trong khi nhiệm vụ thứ hai đóng góp cho mỗi ngày từ 1 đến 6. Kết quả đúng là$1,2,1,2,1,2$. Bất kỳ giải pháp nào xử lý sai$a_i=0$có thể cố gắng truyền bá nó không chính xác hoặc gặp sự cố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu từ việc giải thích vấn đề theo nghĩa đen. Đối với mỗi nhiệm vụ, chúng tôi mô phỏng tất cả các lần xuất hiện trong tương lai của nó bằng cách duyệt qua bội số thời gian của nó. Đối với mỗi bội số$d = a_i, 2a_i, 3a_i, \dots$lên đến$k$, chúng tôi tăng bộ đếm cho ngày$d$. 

Điều này đúng vì nó trực tiếp xây dựng sự đóng góp chính xác của mọi nhiệm vụ. Tuy nhiên, chi phí trở thành vấn đề khi nhiều nhiệm vụ có giá trị nhỏ của$a_i$. Nếu như$a_i = 1$đối với mọi công việc, mỗi công việc đóng góp hàng ngày, tạo ra$O(nk)$cập nhật. Với$n = k = 10^5$, điều này dẫn đến$10^{10}$tăng dần, vượt xa giới hạn khả thi. 

Nhận xét quan trọng là đây là mô hình “tích lũy giống như sàng” cổ điển. Thay vì mô phỏng từng nhiệm vụ một cách độc lập, chúng tôi tổng hợp các nhiệm vụ theo thời gian của chúng. Nếu chúng ta đếm có bao nhiêu nhiệm vụ có cùng giá trị$a$, thì mỗi cái khác nhau$a$đóng góp nhiều lần với bội số của$a$. Điều này cho phép chúng tôi chuyển từ mô phỏng theo từng nhiệm vụ sang tích lũy tần số theo từng giá trị. 

Khi chúng ta có một mảng tần số$freq[a]$, chúng ta chỉ cần lặp lại các kích thước bước có thể$a$và với mỗi một phân phối phần đóng góp của nó cho tất cả các bội số. Điều này biến công việc lặp đi lặp lại thành công việc được nhóm, tương tự như cách tối ưu hóa các vấn đề về số chia hoặc sàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$trường hợp xấu nhất |$O(k)$| Quá chậm | 
| Tối ưu (tần số + bội số) |$O(k \log k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi danh sách các nhiệm vụ thành bảng tần số gồm các khoảng thời gian lặp lại. Sau đó, chúng tôi tuyên truyền các khoản đóng góp một cách hiệu quả theo bội số. 

1. Xây dựng một mảng`freq`Ở đâu`freq[x]`lưu trữ chính xác bao nhiêu nhiệm vụ có thời gian lặp lại`x`. Điều này nhóm các hành vi giống hệt nhau lại với nhau, tránh làm việc dư thừa. 
2. Tạo mảng đáp án`ans`kích thước`k`, khởi tạo bằng 0. Mỗi vị trí`ans[d]`sẽ lưu trữ bao nhiêu nhiệm vụ xảy ra trong ngày`d`. 
3. Đối với mỗi giá trị kỳ có thể`x`từ 1 đến`k`, nếu như`freq[x] > 0`, chúng tôi phân phối phần đóng góp của nó cho tất cả bội số của`x`trong phạm vi. Điều này có nghĩa là lặp đi lặp lại`d = x, 2x, 3x, ... ≤ k`và thêm`freq[x]`ĐẾN`ans[d]`. 
4. Bỏ qua mọi nhiệm vụ với`a_i = 0`, vì chúng không bao giờ đóng góp các lần lặp lại trong tương lai. 
5. Xuất mảng`ans[1..k]`. 

Mỗi lớp thời kỳ hoạt động giống như một “làn sóng” truyền qua dòng thời gian theo những khoảng thời gian cố định. Việc nhóm chúng lại đảm bảo mỗi đợt được xử lý một lần thay vì một lần cho mỗi tác vụ. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày nào$d$, một nhiệm vụ có dấu chấm$x$đóng góp chính xác khi nào$d$là bội số của$x$. Thuật toán đảm bảo rằng với mỗi$x$, chúng ta cộng tần số của nó với mọi bội số như vậy. Vì mọi tác vụ được tính chính xác một lần trong nhóm tần số của nó và được thêm chính xác vào những ngày hợp lệ của nó nên không xảy ra tình trạng đếm quá mức hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = [0] * (k + 1)
    
    for x in a:
        if x != 0 and x <= k:
            freq[x] += 1
    
    ans = [0] * (k + 1)
    
    for x in range(1, k + 1):
        if freq[x] == 0:
            continue
        for d in range(x, k + 1, x):
            ans[d] += freq[x]
    
    print(*ans[1:k+1])

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đếm xem có bao nhiêu nhiệm vụ được chia sẻ trong mỗi khoảng thời gian lặp lại. Séc`x != 0`tránh các kích thước bước không hợp lệ, vì số 0 không tạo ra cấp số cộng hợp lệ của ngày. Điều kiện thứ hai`x <= k`đảm bảo chúng tôi chỉ xử lý các khoảng thời gian thực sự có thể xuất hiện trong khoảng thời gian. 

Cấu trúc vòng lặp lồng nhau là bước lan truyền cốt lõi. Đối với mỗi thời kỳ`x`, chúng ta truy cập bội số của nó và tăng các bộ đếm ngày tương ứng. Điều này phản ánh định nghĩa toán học về sự xuất hiện định kỳ. 

Một điểm tinh tế là lập chỉ mục: mảng câu trả lời có kích thước`k+1`để chỉ số ngày căn chỉnh tự nhiên với vị trí mảng, tránh sai sót từng lỗi một khi viết`ans[d]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10
1 2
```Chúng tôi xây dựng tần số: 

| x | tần số[x] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Bây giờ tuyên truyền đóng góp: 

| x | ngày cập nhật | 
| --- | --- | 
| 1 | 1 2 3 4 5 6 7 8 9 10 | 
| 2 | 2 4 6 8 10 | 

Tích lũy cuối cùng: 

| Ngày | Đóng góp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 1 | 
| 6 | 2 | 
| 7 | 1 | 
| 8 | 2 | 
| 9 | 1 | 
| 10 | 2 | 

Đầu ra:```
1 2 1 2 1 2 1 2 1 2
```Dấu vết này cho thấy các mẫu tuần hoàn chồng chéo tích lũy tuyến tính như thế nào thông qua các bội số chung. 

### Ví dụ 2 

đầu vào:```
2 6
0 1
```Bảng tần số: 

| x | tần số[x] | 
| --- | --- | 
| 1 | 1 | 

Nhiệm vụ có khoảng thời gian bằng 0 sẽ bị bỏ qua. 

Tuyên truyền cho x = 1: 

| x | ngày cập nhật | 
| --- | --- | 
| 1 | 1 2 3 4 5 6 | 

Mảng cuối cùng: 

| Ngày | Giá trị | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 

Đầu ra:```
1 1 1 1 1 1
```Điều này xác nhận việc xử lý chính xác trường hợp 0 ​​và sự lan truyền thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \log k)$| Mỗi chu kỳ đóng góp theo bội số của nó, tạo thành một chuỗi hài trên tất cả x | 
| Không gian |$O(k)$| Mảng tần số và câu trả lời có kích thước k | 

Thời gian chạy phù hợp thoải mái trong giới hạn vì tổng hài trên bội số được giới hạn bởi$k \log k$, Và$k \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (k + 1)
    for x in a:
        if x != 0 and x <= k:
            freq[x] += 1

    ans = [0] * (k + 1)
    for x in range(1, k + 1):
        for d in range(x, k + 1, x):
            ans[d] += freq[x]

    return " ".join(map(str, ans[1:k+1]))

# provided samples
assert run("2 10\n1 2") == "1 2 1 2 1 2 1 2 1 2"
assert run("2 6\n0 1") == "1 1 1 1 1 1"
assert run("3 15\n1 2 3") == "1 2 2 3 1 3 1 3 2 2 1 4 1 2 2"

# custom cases
assert run("1 5\n0") == "0 0 0 0 0", "no propagation"
assert run("3 5\n1 1 1") == "3 3 3 3 3", "all identical period"
assert run("2 7\n5 10") == "0 0 0 0 1 0 0", "large periods"
assert run("4 8\n2 2 2 2") == "0 4 0 4 0 4 0 4", "even-only spikes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 0`| tất cả số không | xử lý khoảng thời gian bằng 0 | 
|`3 5 / 1 1 1`| tải tối đa không đổi | lặp đi lặp lại thời kỳ giống hệt nhau | 
|`2 7 / 5 10`| sự kiện thưa thớt | thời gian vượt quá k | 
|`4 8 / 2 2 2 2`| gai xen kẽ | nhiều đóng góp giống hệt nhau | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị đều bằng 0. Trong tình huống này, sẽ không có sự lan truyền nào xảy ra và đầu ra phải hoàn toàn bằng 0. Bảng tần số loại trừ các số 0, do đó vòng lặp lồng nhau không bao giờ thêm bất kỳ đóng góp nào. 

Một trường hợp khác là khi tất cả các giá trị đều bằng 1. Mọi tác vụ đều đóng góp hàng ngày và thuật toán tích lũy tất cả tần số vào mọi vị trí. Từ`freq[1] = n`, mỗi ngày trở thành`n`. 

Khi giá trị vượt quá`k`, chúng không bao giờ xuất hiện dưới dạng chỉ mục hợp lệ trong vòng lan truyền. Họ được bỏ qua một cách an toàn bởi`x <= k`kiểm tra, ngăn chặn việc lặp lại lãng phí và đảm bảo tính chính xác mà không cần xử lý đặc biệt. 

Cuối cùng, các phân phối hỗn hợp như nhiều giá trị nhỏ kết hợp với các giá trị lớn thể hiện ưu điểm của việc nhóm. Các giá trị nhỏ chiếm ưu thế trong thời gian chạy, nhưng việc nhóm đảm bảo mỗi giá trị được xử lý một lần trên mỗi chuỗi chia thay vì trên mỗi tác vụ.
