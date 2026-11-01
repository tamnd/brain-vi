---
title: "CF 106433B - Công trình đôi"
description: "Bài toán mô tả một hành lang chứa các tác phẩm nghệ thuật. Mỗi tác phẩm đều có một mã định danh cũ và một số tác phẩm là anh em sinh đôi vì chúng có chung một mã định danh."
date: "2026-06-25T09:37:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106433
codeforces_index: "B"
codeforces_contest_name: "XXX Spain Olympiad in Informatics, online qualifier"
rating: 0
weight: 106433
solve_time_s: 39
verified: true
draft: false
---

[CF 106433B - Tác phẩm song sinh](https://codeforces.com/problemset/problem/106433/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một hành lang chứa các tác phẩm nghệ thuật. Mỗi tác phẩm đều có một mã định danh cũ và một số tác phẩm là anh em sinh đôi vì chúng có chung một mã định danh. Các mã định danh mới phải bảo toàn chính xác các nhóm này: hai tác phẩm có cùng mã định danh cũ vẫn phải có mã định danh mới giống nhau, trong khi hai mã định danh cũ khác nhau phải nhận các mã định danh mới khác nhau. 

Thứ tự công việc ở hành lang không thể thay đổi. Người kiểm tra đọc các mã định danh mới từ trái sang phải và một cặp vị trí được coi là xấu khi tác phẩm trước đó có mã định danh lớn hơn tác phẩm sau. Nhiệm vụ là gán các mã định danh mới sao cho số lần đảo ngược này càng nhỏ càng tốt. Bất kỳ bài tập nào có số lần đảo ngược tối thiểu đều được chấp nhận. 

Hạn chế chính là mọi số nhận dạng cũ xuất hiện nhiều nhất hai lần. Tổng số công việc trên tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, do đó nghiệm phải gần tuyến tính. Việc sắp xếp toàn bộ mảng có thể chấp nhận được, nhưng so sánh từng cặp tác phẩm thì không. Một phương pháp bậc hai sẽ đạt được khoảng$10^{10}$hoạt động cho đầu vào lớn nhất, vượt xa giới hạn một giây thông thường. 

Các trường hợp phức tạp đến từ các số nhận dạng trùng lặp xuất hiện ở các vị trí khác nhau. Một giải pháp bất cẩn có thể gán các mã định danh chỉ theo lần xuất hiện đầu tiên hoặc theo giá trị số của mã cũ, nhưng các giá trị ban đầu không có ý nghĩa gì sau khi nhóm được cố định. 

Ví dụ:```
1
3
3 2 1
```Đầu ra đúng có thể là:```
1 2 3
```Một giải pháp giữ nguyên giá trị ban đầu sẽ xuất ra`3 2 1`, tạo ra sự đảo ngược không cần thiết. Các nhãn cũ chỉ mô tả tác phẩm nào là song sinh. 

Một trường hợp khác là:```
1
4
2 1 1 2
```Đầu ra đúng có thể là:```
1 2 2 1
```Hai cặp phải ở bên nhau. Trao cái đầu tiên`2`một giá trị khác với giá trị thứ hai`2`sẽ vi phạm yêu cầu sinh đôi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi thứ tự có thể có của các nhóm và gán các số mới theo thứ tự đó. Ý tưởng này đúng vì khi các nhóm được sắp xếp theo thứ tự tăng dần thì sự nghịch đảo sẽ được giảm thiểu. Tuy nhiên, với nhiều nhóm riêng biệt, việc thử tất cả các đơn hàng là giai thừa và trở nên bất khả thi ngay lập tức. 

Một lực lượng vũ phu thực tế hơn sẽ liên tục đếm số lần đảo ngược trong khi thử các nhiệm vụ khác nhau. Ngay cả khi chúng ta chỉ xem xét một tập hợp nhỏ các phép gán có thể, việc đếm tất cả các cặp sẽ yêu cầu$O(n^2)$công việc. Với$n = 10^5$, đó là về$10^{10}$so sánh. 

Điều quan trọng là chúng ta không cần phải tìm kiếm các bài tập. Mỗi mã định danh cũ đại diện cho một nhóm không thể chia cắt. Nếu chúng ta chọn thứ tự của các nhóm này, gán các số mới liên tiếp theo thứ tự đó thì số lần đảo ngược giữa các nhóm chỉ phụ thuộc vào thứ tự tương đối của các nhóm đó ở hành lang. Để giảm thiểu sự đảo ngược, các nhóm phải xuất hiện theo thứ tự đánh số mới theo lần xuất hiện đầu tiên từ trái sang phải. 

Hãy tưởng tượng đang quét hành lang. Nhóm mới gặp đầu tiên sẽ nhận được mã`1`, bởi vì việc đặt bất kỳ nhóm nào khác trước nó sẽ tạo ra sự đảo ngược với lần xuất hiện đầu tiên này. Nhóm chưa được nhìn thấy tiếp theo sẽ nhận được mã tiếp theo, v.v. Các lần xuất hiện lặp đi lặp lại sẽ sử dụng lại mã đã được gán cho nhóm đó. 

Điều này có hiệu quả vì mỗi nhóm có thể được coi là một đối tượng duy nhất. Việc gán một mã nhỏ hơn cho một nhóm xuất hiện lần đầu tiên sớm hơn không bao giờ tạo ra nhiều sự đảo ngược hơn việc hoán đổi mã đó với một nhóm có lần xuất hiện đầu tiên muộn hơn. Việc lặp lại lập luận trao đổi này sẽ dẫn đến thứ tự xuất hiện lần đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ánh xạ trống từ mã cũ sang mã mới. Ánh xạ đại diện cho các nhóm sinh đôi được bảo tồn. Mỗi lần xuất hiện của cùng một mã cũ phải nhận được cùng một giá trị. 
2. Quét tác phẩm từ trái sang phải. Khi mã cũ xuất hiện lần đầu tiên, hãy gán mã mới có sẵn tiếp theo cho mã đó. Điều này tuân theo thứ tự các nhóm xuất hiện một cách tự nhiên ở hành lang. 
3. Khi mã cũ xuất hiện trở lại, hãy sử dụng lại mã mới đã gán trước đó. Lần xuất hiện thứ hai không thể ảnh hưởng đến thứ tự vì nhóm của nó đã được đặt. 
4. Xuất mã được chỉ định theo thứ tự hành lang ban đầu. 

Tại sao nó hoạt động: 

Sự xuất hiện đầu tiên của mỗi nhóm xác định vị trí sớm nhất mà nhóm đó có thể ảnh hưởng đến sự đảo ngược. Giả sử hai nhóm A và B có lần xuất hiện đầu tiên trong đó A xuất hiện trước B. Việc gán cho B một mã mới nhỏ hơn A sẽ tạo ra sự đảo ngược giữa những lần xuất hiện đầu tiên đó. Việc hoán đổi mã mới của chúng sẽ loại bỏ sự đảo ngược đó và không gây hại cho bất kỳ cặp nào liên quan đến những lần xuất hiện sau đó vì tất cả các thành viên trong một nhóm đều di chuyển cùng nhau. Áp dụng đối số này nhiều lần có nghĩa là thứ tự tối ưu chính xác là thứ tự xuất hiện đầu tiên của các nhóm. Thuật toán chỉ định chính xác thứ tự đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mapping = {}
        nxt = 1
        res = []

        for x in a:
            if x not in mapping:
                mapping[x] = nxt
                nxt += 1
            res.append(mapping[x])

        ans.append(" ".join(map(str, res)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ mối quan hệ giữa số nhận dạng cũ và số nhận dạng mới. Lần đầu tiên một giá trị xuất hiện, mã sẽ gán số tiếp theo. Đây là thời điểm duy nhất cần đưa ra quyết định. 

Thứ tự quét là chi tiết triển khai chính. Nếu mảng được sắp xếp trước, thứ tự hành lang ban đầu sẽ bị mất và nhiệm vụ được tạo ra có thể làm tăng số lần đảo ngược. Thuật toán chỉ dán nhãn lại, nó không bao giờ sắp xếp lại tác phẩm. 

Biến`nxt`bắt đầu từ một vì tất cả các số nhận dạng mới phải dương. Số nguyên Python được sử dụng một cách tự nhiên ở đây, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
4
2 1 1 2
```dấu vết là: 

| Vị trí | Mã cũ | Lập bản đồ | Mã tiếp theo | Được giao | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 → 1 | 2 | 1 | 
| 2 | 1 | 1 → 2 | 3 | 2 | 
| 3 | 1 | 1 → 2 | 3 | 2 | 
| 4 | 2 | 2 → 1 | 3 | 1 | 

Đầu ra là:```
1 2 2 1
```Dấu vết cho thấy sự xuất hiện lần thứ hai của mỗi nhóm sinh đôi không tạo ra định danh mới. 

Đối với đầu vào:```
1
6
5 5 3 7 3 7
```dấu vết là: 

| Vị trí | Mã cũ | Lập bản đồ | Mã tiếp theo | Được giao | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 → 1 | 2 | 1 | 
| 2 | 5 | 5 → 1 | 2 | 1 | 
| 3 | 3 | 3 → 2 | 3 | 2 | 
| 4 | 7 | 7 → 3 | 4 | 3 | 
| 5 | 3 | 3 → 2 | 4 | 2 | 
| 6 | 7 | 7 → 3 | 4 | 3 | 

Đầu ra là:```
1 1 2 3 2 3
```Điều này chứng tỏ rằng các giá trị số của số nhận dạng cũ không còn phù hợp. Chỉ có thứ tự xuất hiện lần đầu mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mọi công việc đều được xử lý một lần và các thao tác từ điển được mong đợi là O(1). | 
| Không gian | O(n) | Ánh xạ lưu trữ một mục nhập cho mỗi mã định danh cũ riêng biệt. | 

Tổng số công việc trên tất cả các trường hợp thử nghiệm là$10^5$, do đó nghiệm tuyến tính dễ dàng phù hợp với các giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính vì từ điển chứa nhiều nhất một mục nhập cho mỗi tác phẩm. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""2
3
3 2 1
4
2 1 1 2
""") == """1 2 3
1 2 2 1
""", "samples"

assert run("""1
1
10
""") == """1
""", "single element"

assert run("""1
5
4 4 4 4 4
""") == """1 1 1 1 1
""", "all equal"

assert run("""1
6
6 5 6 4 5 4
""") == """1 2 1 3 2 3
""", "interleaved twins"

assert run("""1
5
1 2 3 4 5
""") == """1 2 3 4 5
""", "all different"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 1`|`1 2 3`| Nhóm phải tuân theo hình thức ban đầu chứ không phải giá trị cũ. | 
|`2 1 1 2`|`1 2 2 1`| Lần xuất hiện đôi phải nhận được mã giống hệt nhau. | 
|`10`|`1`| Trường hợp kích thước tối thiểu. | 
|`4 4 4 4 4`|`1 1 1 1 1`| Tất cả các yếu tố thuộc về một nhóm. | 
|`6 5 6 4 5 4`|`1 2 1 3 2 3`| Xử lý các nhóm trùng lặp xen kẽ. | 

## Vỏ cạnh 

Dành cho:```
1
3
3 2 1
```thuật toán bắt đầu với một bản đồ trống. Nó nhìn thấy`3`đầu tiên và gán mã`1`, sau đó`2`nhận được mã`2`, Và`1`nhận được mã`3`. Kết quả là`1 2 3`, loại bỏ các đảo ngược được tạo bởi các nhãn ban đầu. 

Vì:```
1
4
2 1 1 2
```cái đầu tiên`2`nhận được mã`1`, và cái đầu tiên`1`nhận được mã`2`. Khi thuật toán đạt đến vị trí thứ ba, nó thấy rằng`1`đã tồn tại trên bản đồ nên nó sử dụng lại mã`2`. trận chung kết`2`cũng sử dụng lại mã`1`. Kết quả giữ cho cả hai cặp sinh đôi còn nguyên vẹn. 

Vì:```
1
5
7 7 7 7 7
```chỉ có một nhóm. Vị trí đầu tiên tạo mục nhập ánh xạ duy nhất và mọi vị trí sau đó sẽ sử dụng lại mục nhập đó. Đầu ra là:```
1 1 1 1 1
```Không có nhiệm vụ nào khác có thể cải thiện điều này vì không có nhóm riêng biệt nào để sắp xếp lại.
