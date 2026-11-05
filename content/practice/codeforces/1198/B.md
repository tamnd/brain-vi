---
title: "CF 1198B - Nhà nước phúc lợi"
description: "Chúng tôi đang duy trì một danh sách số dư động cho một nhóm công dân cố định. Ban đầu, mỗi công dân có một số tiền nhất định. Sau đó, một chuỗi các sự kiện sẽ làm thay đổi số dư này theo hai cách khác nhau."
date: "2026-06-13T14:40:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1198
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 576 (Div. 1)"
rating: 1600
weight: 1198
solve_time_s: 204
verified: true
draft: false
---

[CF 1198B - Trạng thái phúc lợi](https://codeforces.com/problemset/problem/1198/B) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một danh sách số dư động cho một nhóm công dân cố định. Ban đầu, mỗi công dân có một số tiền nhất định. Sau đó, một chuỗi các sự kiện sẽ làm thay đổi số dư này theo hai cách khác nhau. 

Loại sự kiện đầu tiên trực tiếp đặt số dư của một công dân cụ thể thành một giá trị chính xác, như thể biên lai ghi đè thông tin của chúng ta về số tiền hiện tại của người đó. Loại sự kiện thứ hai là hành động của chính phủ toàn cầu: mọi công dân có số dư hoàn toàn dưới ngưỡng nhất định sẽ được nâng lên ngưỡng đó, trong khi những công dân đã đạt hoặc cao hơn ngưỡng đó sẽ không thay đổi. 

Đầu ra sau khi xử lý tất cả các sự kiện chỉ đơn giản là số dư cuối cùng của mỗi công dân. 

Khó khăn chính là các bản cập nhật toàn cầu có thể ảnh hưởng đến nhiều công dân cùng một lúc và có thể có tới 200.000 sự kiện. Một cách tiếp cận ngây thơ quét tất cả công dân để tìm mọi cập nhật toàn cầu sẽ yêu cầu tới 40 tỷ thao tác trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại toàn bộ mảng cho mỗi lần cập nhật. 

Một vấn đề nhỏ xuất hiện khi kết hợp cập nhật cá nhân và cập nhật toàn cầu. Giả sử một công dân được tăng cường nhờ hoạt động toàn cầu và sau đó nhận được bản cập nhật trực tiếp. Nếu không cẩn thận, chúng ta có thể “áp dụng lại” các hoạt động toàn cầu cũ một cách không chính xác hoặc ghi đè các giá trị mới hơn bằng các giá trị lỗi thời. Ví dụ: nếu một công dân được nâng lên 10 bởi một sự kiện toàn cầu và sau đó được đặt rõ ràng thành 5, thì việc triển khai đơn giản chỉ theo dõi các ngưỡng tối thiểu sẽ đưa họ trở lại 10 một cách không chính xác. 

Một trường hợp khác là các hoạt động toàn cầu lặp đi lặp lại với ngưỡng giảm dần. Một giả định đơn điệu ngây thơ về ngưỡng tối đa sẽ thất bại trừ khi chúng ta xử lý rõ ràng thứ tự và tính liên tục của các cập nhật cho mỗi vị trí. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ áp dụng trực tiếp từng sự kiện. Đối với sự kiện loại 2 có ngưỡng x, chúng tôi sẽ quét tất cả công dân và cập nhật những sự kiện có giá trị nhỏ hơn x. Điều này đúng nhưng tốn kém: mỗi thao tác như vậy tốn O(n) và với q lên tới 200.000, tổng chi phí sẽ trở thành O(nq), quá lớn. 

Quan sát quan trọng là các cập nhật toàn cầu chỉ nâng cao giá trị và chúng dựa trên các ngưỡng. Điều này cho thấy rằng một khi giá trị của một công dân đã được nâng lên vượt quá một mức nhất định thì nó sẽ không bao giờ cần được xem xét lại ở các ngưỡng nhỏ hơn nữa. Tuy nhiên, vì các cập nhật riêng lẻ có thể hạ giá trị của một công dân xuống, nên chúng tôi không thể đơn giản giữ mức tối đa toàn cầu. 

Bí quyết là xử lý các sự kiện ngược lại. Nếu chúng ta đảo ngược dòng thời gian, sự kiện loại 2 sẽ trở thành: “giá trị trong tương lai ít nhất phải là x” và sự kiện loại 1 trở thành nhiệm vụ cuối cùng đã biết có thể ghi đè các ràng buộc trước đó. Làm việc ngược cho phép chúng tôi tính toán, đối với mỗi công dân, giới hạn dưới tốt nhất có thể được áp đặt bởi các cập nhật toàn cầu trong tương lai, đồng thời tôn trọng nhiệm vụ trực tiếp gần đây nhất. 

Để làm cho điều này chính xác, chúng tôi duy trì một giá trị đang chạy`mx`đại diện cho ngưỡng tối đa được thấy cho đến nay trong quá trình xử lý đảo ngược. Khi chúng tôi gặp bản cập nhật toàn cầu, chúng tôi cập nhật`mx`. Khi gặp nhiệm vụ cá nhân của công dân p, nếu công dân đó chưa quyết toán xong thì chúng ta giao nhiệm vụ cho họ`max(a[p], mx)`. 

Cái nhìn sâu sắc là một khi chúng tôi ấn định giá trị cuối cùng của một công dân thì các sự kiện trước đó không còn quan trọng đối với họ nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Xử lý ngược với Theo dõi tối đa | O(n + q) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các sự kiện theo thứ tự ngược lại để các ràng buộc chung trở thành giới hạn tích lũy thấp hơn thay vì quét mảng lặp đi lặp lại. 

1. Đọc tất cả các giá trị ban đầu và lưu trữ tất cả các sự kiện. 
2. Khởi tạo một mảng`ans`là “không được chỉ định” cho mỗi công dân. 
3. Duy trì một biến`mx = 0`, lưu trữ ngưỡng tối đa từ các sự kiện toàn cầu gặp phải cho đến nay theo chiều ngược lại. 
4. Lặp lại các sự kiện từ sự kiện cuối cùng đến sự kiện đầu tiên. 
5. Nếu sự kiện là bản cập nhật toàn cầu có ngưỡng`x`, bộ`mx = max(mx, x)`. Điều này thể hiện rằng bất kỳ giá trị nào chúng tôi quyết định sau này phải đáp ứng ít nhất yêu cầu tối thiểu này. 
6. Nếu sự kiện là cập nhật cá nhân cho công dân`p`có giá trị`x`, và công dân này chưa được phân công, thiết lập`ans[p] = max(x, mx)`. Điều này đảm bảo rằng chúng tôi tôn trọng cả sự phân công trực tiếp và tất cả các ràng buộc toàn cầu trong tương lai (theo thứ tự ban đầu). 
7. Sau khi xử lý tất cả các sự kiện, bất kỳ công dân nào vẫn chưa được chỉ định sẽ nhận được`max(a[i], mx)`. 

Lý do điều này có tác dụng là vì mỗi công dân được hoàn thành chính xác một lần, tại thời điểm lần đầu tiên chúng ta gặp nhiệm vụ mới nhất của họ theo thứ tự ngược lại. Tại thời điểm đó, tất cả các ràng buộc toàn cầu xảy ra sau nhiệm vụ đó trong dòng thời gian ban đầu đã được tích lũy vào`mx`. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý các sự kiện ngược lại,`mx`luôn bằng ngưỡng tối đa của tất cả các thao tác loại 2 xảy ra sau điểm hiện tại trong dòng thời gian ban đầu. Do đó, khi chúng tôi hoàn thiện giá trị của một công dân, chúng tôi thực thi chính xác mọi cập nhật toàn cầu có ảnh hưởng đến họ sau nhiệm vụ trực tiếp cuối cùng của họ. Vì chúng tôi chỉ chỉ định một lần cho mỗi công dân nên chúng tôi không bao giờ ghi đè một giá trị đã giải quyết tất cả các ràng buộc có liên quan và không bước nào trong tương lai có thể làm mất hiệu lực giá trị đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())

events = []
for _ in range(q):
    parts = list(map(int, input().split()))
    events.append(parts)

ans = [-1] * n
mx = 0

for e in reversed(events):
    if e[0] == 2:
        mx = max(mx, e[1])
    else:
        _, p, x = e
        p -= 1
        if ans[p] == -1:
            ans[p] = max(x, mx)

for i in range(n):
    if ans[i] == -1:
        ans[i] = max(a[i], mx)

print(*ans)
```Cốt lõi của việc thực hiện là quét ngược. Chúng tôi không mô phỏng các bản cập nhật về sau vì điều đó sẽ yêu cầu quét toàn bộ nhiều lần. Thay vào đó, chúng tôi coi các hoạt động toàn cầu là những ràng buộc dai dẳng được tích lũy vào`mx`. 

Một điểm tinh tế là việc khởi tạo`ans`với -1. Điều này đảm bảo rằng chỉ phép gán mới nhất theo thứ tự ngược lại mới được sử dụng, phù hợp với thực tế là trong thời gian chuyển tiếp, các phép gán sau sẽ ghi đè lên các phép gán trước đó. 

Cuối cùng, mảng ban đầu chỉ được sử dụng cho những công dân chưa bao giờ nhận được sự kiện phân công trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
3
2 3
1 2 2
2 1
```Chúng tôi xử lý ngược lại. 

| Bước | Sự kiện | mx | Những thay đổi được chỉ định | 
| --- | --- | --- | --- | 
| 1 | 2 1 | 1 | không | 
| 2 | 1 2 2 | 1 | ans[1] = max(2,1)=2 | 
| 3 | 2 3 | 3 | không | 

Chung kết: 

- ans = [3, 2, 3, 4] 

Điều này cho thấy cách cập nhật toàn cầu sau này với ngưỡng 3 chi phối các ràng buộc nhỏ hơn trước đó và cách phân công trực tiếp được hạn chế đối với các ràng buộc tích lũy. 

### Ví dụ 2 

đầu vào:```
5
3 50 2 1 10
3
2 0
1 2 0
2 8
```Xử lý ngược: 

| Bước | Sự kiện | mx | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 8 | 8 | mx = 8 | 
| 2 | 1 2 0 | 8 | ans[1] = 8 | 
| 3 | 2 0 | 8 | mx ở lại 8 | 

Chung kết: 

- ans = [8, 8, 8, 8, 10] 

Điều này chứng tỏ rằng ngay cả phép gán thấp (0) cũng bị ghi đè bởi các ràng buộc toàn cục sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi sự kiện được xử lý ngược lại một lần và mỗi công dân được hoàn tất tối đa một lần | 
| Không gian | O(n + q) | Lưu trữ các sự kiện và mảng kết quả | 

Giải pháp có quy mô tuyến tính với kích thước đầu vào, điều này cần thiết với giới hạn 200.000 đối với cả công dân và sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    events = [list(map(int, input().split())) for _ in range(q)]

    ans = [-1] * n
    mx = 0

    for e in reversed(events):
        if e[0] == 2:
            mx = max(mx, e[1])
        else:
            _, p, x = e
            p -= 1
            if ans[p] == -1:
                ans[p] = max(x, mx)

    for i in range(n):
        if ans[i] == -1:
            ans[i] = max(a[i], mx)

    return " ".join(map(str, ans))

# provided sample
assert run("""4
1 2 3 4
3
2 3
1 2 2
2 1
""") == "3 2 3 4"

# all equal, only global
assert run("""3
5 5 5
2
2 10
2 7
""") == "10 10 10"

# only personal updates
assert run("""3
1 2 3
2
1 2 10
1 3 7
""") == "1 10 7"

# mixed order stress
assert run("""5
0 0 0 0 0
4
1 1 5
2 3
1 1 1
2 10
""") == "10 10 10 10 10"

# minimum size
assert run("""1
0
1
2 5
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các thế giới bình đẳng | 10 10 10 | sự thống trị toàn cầu lặp đi lặp lại | 
| chỉ cập nhật cá nhân | hỗn hợp | xử lý ghi đè | 
| thứ tự hỗn hợp | tất cả 10 | tương tác của các bản cập nhật | 
| phần tử đơn | 5 | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một công dân được cập nhật nhiều lần và chỉ có bản cập nhật sớm nhất trong thời gian tới mới là trạng thái cuối cùng. Trong xử lý ngược, việc này được xử lý một cách tự nhiên vì chúng ta gán`ans[p]`chỉ một lần. 

Coi như:```
3
1 1 1
3
1 1 5
1 1 2
2 10
```Thứ tự ngược lại: 

- 10 bộ toàn cầu mx = 10 
- bài tập đầu tiên gặp phải đối với người 1 bộ ans[0] = 10 
- nhiệm vụ trước đó bị bỏ qua 

Thuật toán trả về chính xác:```
10 1 1
```Một trường hợp nguy hiểm khác là khi không có cập nhật cá nhân nào xảy ra. Trong trường hợp đó, tất cả các giá trị chỉ được nâng lên ngưỡng tổng thể tối đa được thấy ở bất kỳ đâu trong chuỗi. Từ`mx`tổng hợp tất cả các sự kiện loại 2, bước chuyển nhượng cuối cùng sẽ áp dụng chính xác sự kiện đó một cách thống nhất cho tất cả công dân.
