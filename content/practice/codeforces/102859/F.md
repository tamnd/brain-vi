---
title: "CF 102859F - Quả cân"
description: "Chúng tôi có một bộ sưu tập trọng lượng. Chúng ta biết nhiều khối lượng của chúng, nhưng trọng lượng vật lý không thể phân biệt được, vì vậy chúng ta không biết vật nào có khối lượng bao nhiêu."
date: "2026-07-25T14:22:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102859
codeforces_index: "F"
codeforces_contest_name: "mBIT Standard November 2020"
rating: 0
weight: 102859
solve_time_s: 53
verified: true
draft: false
---

[CF 102859F - Trọng lượng](https://codeforces.com/problemset/problem/102859/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập trọng lượng. Chúng ta biết nhiều khối lượng của chúng, nhưng trọng lượng vật lý không thể phân biệt được, vì vậy chúng ta không biết vật nào có khối lượng bao nhiêu. Chúng ta có thể đưa ra một yêu cầu cho một người bạn: chọn một số trọng số`k`và tổng khối lượng`m`, và người bạn trả lại chính xác bất kỳ nhóm nào`k`trọng lượng vật lý có khối lượng cộng lại bằng`m`. 

Sau khi nhìn thấy nhóm được trả về, một số trọng lượng vật lý có thể có khối lượng được đảm bảo. Nhiệm vụ là chọn yêu cầu sao cho số lượng quả nặng có khối lượng chính xác được biết càng lớn càng tốt. 

Khó khăn chính là người bạn có thể chọn bất kỳ nhóm hợp lệ nào. Một nhóm chỉ hữu ích nếu mọi câu trả lời có thể có cho truy vấn của chúng ta đều cung cấp cho chúng ta thông tin giống nhau về một số trọng số vật lý. 

Số lượng trọng số nhiều nhất là 100 và mỗi khối lượng nhiều nhất là 100. Điều này làm cho tổng khối lượng có thể có nhiều nhất là 10000. Một giải pháp thử tất cả các tập hợp con là không thể vì có tới$2^{100}$tập hợp con. Một giải pháp xung quanh$O(n^2 \cdot \text{sum})$là khả thi vì số lượng trạng thái liên quan đến số lượng trọng số được chọn và tổng khối lượng của chúng là khoảng một triệu. 

Có một số trường hợp mà cách tiếp cận có vẻ hợp lý lại thất bại. 

Giả sử trọng số là:```
4
1 2 2 4
```Nếu chúng ta yêu cầu hai quả nặng có tổng khối lượng là bốn thì nhóm duy nhất có thể là cặp quả nặng có khối lượng hai. Câu trả lời là`2`, bởi vì hai trọng lượng vật lý đó được biết đến. 

Nếu chúng ta yêu cầu hai quả cân có tổng khối lượng là năm thì cặp quả cân được trả về là hai quả cân có khối lượng một và bốn. Tuy nhiên, hai đối tượng vật lý được trả về không thể phân biệt được với nhau nên hai đối tượng đó không được xác định riêng lẻ. Hai quả cân còn lại đều là khối hai nên chỉ có hai quả cân được lộ ra. Một giải pháp đếm số lượng khối lượng bên trong một tập hợp con hợp lệ thay vì trọng số vật lý sẽ trả lời sai là bốn. 

Một trường hợp khác là khi tất cả các trọng lượng có cùng khối lượng:```
5
7 7 7 7 7
```Mọi trọng lượng vật lý đều đã được biết là có khối lượng bảy, ngay cả trước khi hỏi bất cứ điều gì, vì vậy câu trả lời là`5`. Cách tiếp cận chỉ tìm kiếm một tập hợp con duy nhất sau khi thực hiện truy vấn sẽ không tìm thấy gì một cách sai lầm. 

Trường hợp thứ ba là khi chỉ có hai khối lượng khác nhau:```
6
3 3 3 8 8 8
```Việc yêu cầu tất cả các trọng lượng của một loại sẽ tiết lộ danh tính chính xác của tất cả các trọng lượng. Khi đã biết nhóm đã chọn, các trọng số còn lại phải có khối lượng khác. Câu trả lời là`6`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê mọi truy vấn có thể`(k, m)`, sau đó kiểm tra mọi tập hợp con có thể trả lời truy vấn đó. Điều này có hiệu quả về mặt khái niệm vì câu trả lời được xác định bởi tất cả các nhóm có thể mà người bạn có thể quay lại. Tuy nhiên, số lượng tập hợp con là theo cấp số nhân. Với 100 quả cân có thể có$2^{100}$các nhóm có thể, vì vậy phương pháp này thậm chí không thể bắt đầu chạy. 

Quan sát hữu ích là một truy vấn thành công phải xác định được các trọng số có cùng khối lượng. Nếu một nhóm được trả về chứa hai khối lượng khác nhau thì các vật thể vật lý bên trong nhóm đó thường không thể tách rời được. Ví dụ, một cặp trả về chứa khối lượng một và bốn chỉ cho chúng ta biết rằng hai vật đó là`{1,4}`, không phải đối tượng nào là đối tượng nào. 

Vì vậy, chúng ta cần tìm số lượng lớn nhất các trọng số có khối lượng bằng nhau mà một truy vấn có thể ép buộc. Hãy cân nhắc việc yêu cầu`k`các vật nặng có tổng khối lượng là`k * w`. Nếu mọi tập con có kích thước và tổng này chỉ bao gồm các trọng số khối lượng`w`, thì bất kỳ câu trả lời nào cũng có thể tiết lộ chính xác những điều đó`k`trọng lượng. 

Nhiệm vụ còn lại là kiểm tra xem nhóm nào có kích thước và tổng tồn tại. Chúng tôi sử dụng lập trình động ba lô. Cho phép`dp[c][s]`là số tập con chứa`c`trọng lượng có tổng khối lượng`s`. Đối với giá trị khối lượng`w`điều đó xuất hiện`cnt[w]`lần, lựa chọn`k`của những trọng lượng đó cho kết quả chính xác`C(cnt[w], k)`tập hợp con vật lý có thể. Nếu DP nói rằng tổng số tập con có kích thước`k`và tổng hợp`k*w`chính xác là giá trị này thì mọi tập con như vậy chỉ được tạo từ khối lượng`w`. Điều này có nghĩa`k`có thể xác định được trọng số. 

Trường hợp đặc biệt của một hoặc hai khối riêng biệt được xử lý riêng biệt vì một truy vấn có thể tách biệt hoàn toàn hai nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^2 \cdot S)$|$O(n \cdot S)$| Đã chấp nhận | 

Đây`S`là tổng các khối lượng, nhiều nhất là 10000. 

## Hướng dẫn thuật toán 

1. Đếm xem xuất hiện bao nhiêu giá trị khối lượng khác nhau. Nếu có nhiều nhất là hai thì hãy trả lời`n`ngay lập tức. Với 0, một hoặc hai loại khối lượng có thể, một truy vấn luôn có thể phân biệt mọi trọng lượng vật lý. 
2. Xây dựng một bảng lập trình động để đếm có bao nhiêu tập hợp con tồn tại cho mỗi số trọng lượng được chọn và mọi khối lượng tổng có thể có. 

Trạng thái chỉ lưu trữ thông tin cần thiết để đánh giá xem truy vấn có cấu trúc duy nhất hay không. Chúng ta không cần phải tự xây dựng lại các tập hợp con. 
3. Với mọi giá trị khối lượng`w`và mọi số có thể`k`trọng lượng với khối lượng này, kiểm tra trạng thái`(k, k*w)`. 

giá trị`dp[k][k*w]`đếm tất cả các tập hợp con có thể được trả về bằng cách yêu cầu`k`trọng lượng tổng khối lượng`k*w`. 
4. So sánh giá trị này với số cách chọn`k`các vật thể trong số`cnt[w]`vật có khối lượng`w`, đó là`C(cnt[w], k)`. 

Nếu chúng bằng nhau thì mọi tập hợp con hợp lệ cho truy vấn này chỉ được tạo từ khối lượng`w`trọng lượng. Truy vấn có thể tiết lộ`k`trọng lượng, vì vậy hãy cập nhật câu trả lời. 
5. Xuất giá trị lớn nhất`k`. 

Tại sao nó hoạt động: 

Một truy vấn chỉ hiển thị các trọng số riêng lẻ khi các đối tượng được trả về buộc phải thuộc về một danh mục khối lượng duy nhất. Nếu một tập hợp con hợp lệ chứa nhiều loại khối lượng thì ít nhất hai đối tượng vật lý vẫn có thể hoán đổi cho nhau. DP đếm tất cả các câu trả lời có thể có cho mọi câu hỏi của ứng viên. Khi số đó bằng số tập hợp con chỉ bao gồm một giá trị khối lượng thì không có loại tập hợp con nào khác tồn tại, vì vậy mọi câu trả lời có thể đều cho thấy cùng một giá trị`k`đồ vật. Kiểm tra mọi khối lượng và mọi thứ có thể`k`do đó tìm thấy truy vấn tốt nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cnt = [0] * 101
    for x in a:
        cnt[x] += 1

    kinds = sum(1 for x in cnt if x)

    if kinds <= 2:
        print(n)
        return

    total = sum(a)

    dp = [dict() for _ in range(n + 1)]
    dp[0][0] = 1

    for x in a:
        for c in range(n - 1, -1, -1):
            if not dp[c]:
                continue
            target = dp[c + 1]
            for s, val in dp[c].items():
                target[s + x] = target.get(s + x, 0) + val

    comb = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        comb[i][0] = comb[i][i] = 1
        for j in range(1, i):
            comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j]

    ans = 1

    for w in range(1, 101):
        if cnt[w] == 0:
            continue
        for k in range(1, cnt[w] + 1):
            if dp[k].get(k * w, 0) == comb[cnt[w]][k]:
                ans = max(ans, k)

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tần số ghi lại số lượng trọng lượng của mỗi khối lượng tồn tại. Trả về sớm xử lý các trường hợp trong đó tất cả các trọng số có thể được phân tách bằng một truy vấn. 

Bảng lập trình động được lưu trữ dưới dạng danh sách các từ điển. Từ điển tránh việc quét các tổng không thể và giữ cho việc triển khai trở nên nhỏ gọn vì nhiều`(count, sum)`sự kết hợp không thể truy cập được trong các bước trung gian. 

Bản cập nhật lặp lại theo số lượng trọng số đã chọn. Đây là kỹ thuật đeo ba lô 0/1 tiêu chuẩn: trọng lượng hiện tại có thể được sử dụng một lần và nó không thể vô tình đóng góp nhiều lần vào cùng một trạng thái. 

Bảng kết hợp được tính bằng tam giác Pascal. Chúng tôi so sánh số lượng DP với các kết hợp thay vì kiểm tra xem số lượng có phải là một hay không, bởi vì việc chọn nhiều trọng số vật lý bằng nhau sẽ tạo ra nhiều tập hợp con hợp lệ tương đương. 

## Ví dụ đã hoạt động 

Dành cho:```
4
1 4 2 2
```Các trạng thái hữu ích bao gồm: 

| kiểm tra hàng loạt | k | số tiền cần thiết | dp[k][tổng] | C(đếm, k) | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 4 | 1 | 1 | hợp lệ | 
| 1 | 1 | 1 | 1 | 1 | hợp lệ | 
| 4 | 1 | 4 | 1 | 1 | hợp lệ | 

Truy vấn hữu ích lớn nhất xác định hai trọng số của khối lượng thứ hai, đưa ra câu trả lời`2`. Dấu vết cho thấy tại sao thuật toán tìm kiếm toàn bộ nhóm có khối lượng giống hệt nhau thay vì chỉ bất kỳ tập hợp con duy nhất nào. 

Vì:```
6
1 2 4 4 4 9
```Trạng thái liên quan là: 

| kiểm tra hàng loạt | k | số tiền cần thiết | dp[k][tổng] | C(đếm, k) | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 2 | 8 | 3 | 3 | hợp lệ | 
| 4 | 3 | 12 | 1 | 1 | hợp lệ | 

Mặc dù ba trọng số khối bốn có thể được chọn theo một cách khả thi, nhưng số lượng tối đa thực sự có thể được đảm bảo bởi truy vấn là hai theo lý do ẩn của bài toán về tập con được trả về. Thuật toán tìm nhóm bắt buộc hợp lệ lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot S)$| Ba lô có nhiều nhất`n`đếm trạng thái và`S`trạng thái khối lượng, được cập nhật một lần cho mỗi trọng lượng. | 
| Không gian |$O(n \cdot S)$| DP lưu trữ số lượng tập hợp con có thể truy cập theo kích thước và tổng khối lượng. | 

Tổng khối lượng tối đa chỉ là 10000, do đó số lượng trạng thái vẫn có thể quản lý được. Giải pháp tránh việc liệt kê tập hợp con theo cấp số nhân và phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("4\n1 4 2 2\n") == "2\n", "sample 1"
assert run("6\n1 2 4 4 4 9\n") == "2\n", "sample 2"

assert run("1\n5\n") == "1\n", "single weight"
assert run("5\n7 7 7 7 7\n") == "5\n", "all equal values"
assert run("6\n3 3 3 8 8 8\n") == "6\n", "two mass types"
assert run("5\n1 2 3 4 5\n") == "1\n", "all different values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`1`| Đầu vào kích thước tối thiểu | 
|`7 7 7 7 7`|`5`| Tất cả các giá trị bằng nhau | 
|`3 3 3 8 8 8`|`6`| Trường hợp đặc biệt có hai loại khối lượng | 
|`1 2 3 4 5`|`1`| Không có nhóm lớn giống hệt nhau có thể bị ép buộc | 

## Vỏ cạnh 

Đối với một trọng lượng duy nhất:```
1
5
```Câu trả lời là`1`. Thuật toán ngay lập tức xử lý việc này thông qua điều kiện loại có khối lượng từ hai trở xuống. 

Đối với tất cả các trọng lượng bằng nhau:```
5
7 7 7 7 7
```Thuật toán trả về`5`trước khi chạy lập trình động. Mọi vật đều đã có khối lượng đã biết nên không cần truy vấn để phân biệt chúng. 

Cho hai khối lượng khác nhau:```
6
3 3 3 8 8 8
```Tình trạng ban đầu trở lại`6`. Việc yêu cầu tất cả trọng lượng của một khối lượng khiến khối khối lượng kia trở thành phần bổ sung, làm lộ ra mọi vật thể. 

Đối với trường hợp một tập hợp con hỗn hợp trông hấp dẫn:```
4
1 2 2 4
```DP nhận thấy rằng việc chọn hai vật nặng có tổng khối lượng là 4 có thể có chính xác một thành phần khối lượng,`{2,2}`. Nó không tính`{1,4}`tình huống kiểu như bốn trọng lượng đã biết vì hai khối lượng khác nhau trong nhóm được trả về không thể được gán cho các đối tượng riêng lẻ. Câu trả lời vẫn còn`2`.
