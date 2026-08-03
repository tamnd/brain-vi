---
title: "CF 102599D - Nhà thám hiểm trẻ"
description: "Chúng tôi có một tập hợp các nhà thám hiểm, trong đó mỗi nhà thám hiểm có một giá trị e mô tả số lượng người tối thiểu cần thiết trong bất kỳ nhóm nào có chứa nhà thám hiểm đó. Một nhóm chỉ hợp lệ khi yêu cầu của mọi thành viên được đáp ứng bởi quy mô nhóm cuối cùng."
date: "2026-08-02T06:43:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "D"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 381
verified: false
draft: false
---

[CF 102599D - Nhà thám hiểm trẻ](https://codeforces.com/problemset/problem/102599/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tập hợp các nhà thám hiểm, trong đó mỗi nhà thám hiểm có một giá trị`e`mô tả số lượng người tối thiểu cần có trong bất kỳ nhóm nào có chứa nhà thám hiểm đó. Một nhóm chỉ hợp lệ khi yêu cầu của mọi thành viên được đáp ứng bởi quy mô nhóm cuối cùng. Nhiệm vụ là chọn một số nhà thám hiểm và chia chúng thành số nhóm hợp lệ tối đa có thể. Một số nhà thám hiểm có thể bị bỏ qua nếu không thể xếp chúng vào một nhóm hợp lệ. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp thử nghiệm, mảng biểu thị giá trị thiếu kinh nghiệm của tất cả các nhà thám hiểm. Đầu ra là số lượng nhóm lớn nhất có thể được tạo từ mảng đó. 

Tổng số người khám phá trong tất cả các trường hợp thử nghiệm nhiều nhất là`3 * 10^5`. Điều này có nghĩa là giải pháp phải xử lý mỗi trình thám hiểm chỉ một số lần nhỏ. Một cách tiếp cận thử nhiều cách kết hợp nhóm khác nhau sẽ nhanh chóng trở nên bất khả thi vì số lượng phân vùng có thể tăng theo cấp số nhân. Thậm chí một`O(N^2)`giải pháp sẽ quá chậm khi một trường hợp thử nghiệm đạt đến`200000`những người khám phá, vì vậy giải pháp dự định cần phải gần gũi với`O(N log N)`hoặc`O(N)`. 

Một số trường hợp rất dễ xử lý sai. Nếu mọi nhà thám hiểm đều có yêu cầu`1`, mỗi trình thám hiểm có thể độc lập và câu trả lời là kích thước đầy đủ của mảng. 

Ví dụ:```
1
4
1 1 1 1
```Đầu ra đúng là:```
4
```Một giải pháp bất cẩn luôn cố gắng xây dựng các nhóm lớn nhất có thể trước tiên có thể tạo ra một nhóm có quy mô bốn và mất cơ hội tạo ra bốn nhóm. 

Một trường hợp phức tạp khác là khi các yêu cầu lớn xuất hiện cùng với các yêu cầu nhỏ. 

Ví dụ:```
1
5
2 3 1 2 2
```Đầu ra đúng là:```
2
```Một phương pháp tham lam loại bỏ ngay lập tức những nhà thám hiểm có yêu cầu lớn có thể bỏ lỡ sự thật rằng một nhà thám hiểm có yêu cầu lớn`3`có thể được sử dụng cùng với hai nhà thám hiểm khác để tạo thành một nhóm hợp lệ. 

Trường hợp đặc biệt cuối cùng là khi yêu cầu lớn hơn số lượng trình khám phá hiện có còn lại. 

Ví dụ:```
1
3
1 1 5
```Đầu ra đúng là:```
2
```Không thể bao gồm nhà thám hiểm yêu cầu nhóm năm người, nhưng hai nhà thám hiểm còn lại vẫn có thể tạo thành hai nhóm có quy mô một. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử các cách khác nhau để phân công người khám phá vào các nhóm và giữ được kết quả tốt nhất. Điều này có hiệu quả về mặt khái niệm vì mọi sự sắp xếp hợp lệ có thể đều được xem xét, do đó phải tìm ra cách sắp xếp tốt nhất. Vấn đề là số lượng nhiệm vụ có thể thực hiện tăng lên cực kỳ nhanh chóng. Với`N`nhà thám hiểm, số lượng phân vùng có thể vượt xa những gì có thể kiểm tra được. Ngay cả việc giới hạn việc tìm kiếm ở nhiều ranh giới nhóm có thể cũng sẽ yêu cầu quá nhiều thao tác, đạt tới khoảng`O(N^2)`hoặc tệ hơn trong việc triển khai thực tế. 

Quan sát hữu ích đến từ việc xem xét điều gì khiến một nhóm có giá trị. Nếu một nhóm có kích thước`s`, mọi nhà thám hiểm bên trong nó đều phải có`e <= s`. Điều này có nghĩa là những người khám phá có yêu cầu nhỏ hơn sẽ dễ dàng được bố trí hơn, trong khi những người khám phá có yêu cầu lớn hơn cần nhiều thành viên xung quanh họ hơn. 

Việc sắp xếp các yêu cầu cho phép chúng tôi xử lý các nhà thám hiểm theo thứ tự độ khó tăng dần. Sau khi phân loại, chúng ta có thể xây dựng các nhóm từ yêu cầu nhỏ nhất trở lên. Chúng tôi đếm xem có bao nhiêu nhà thám hiểm đã được thu thập cho nhóm hiện tại. Khi số lượng này đạt đến yêu cầu của người khám phá hiện tại, nhóm có thể được đóng lại và được tính là một nhóm hợp lệ. 

Lý do khiến sự lựa chọn tham lam này có tác dụng là vì một yêu cầu nhỏ không bao giờ có lợi khi chờ đợi một nhóm lớn hơn. Nếu một nhóm đã có đủ người cho người khám phá hiện tại thì việc tạo nhóm sẽ ngay lập tức lưu những người khám phá đó dưới dạng nhóm hoàn chỉnh và để những người khám phá sau này tạo thành các nhóm trong tương lai. Các yêu cầu lớn hơn sẽ được xem xét sau, sau khi có thêm nhiều thành viên tiềm năng trong nhóm. 

Brute-force hoạt động vì nó khám phá tất cả các phân vùng hợp lệ nhưng không thành công vì có quá nhiều khả năng. Quan sát cho thấy thứ tự được sắp xếp hiển thị thời điểm chính xác khi một nhóm trở nên hợp lệ cho phép chúng tôi thay thế việc tìm kiếm phân vùng bằng một lần quét tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn tùy thuộc vào chiến lược tìm kiếm | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(1) bổ sung ngoài việc sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các yêu cầu của người khám phá theo thứ tự không giảm. Các yêu cầu nhỏ hơn nên được xử lý trước vì họ cần ít người hơn để trở thành thành viên hợp lệ của nhóm. 

2. Duyệt mảng đã sắp xếp trong khi vẫn giữ nguyên`current`, số lượng người khám phá hiện được thu thập cho một nhóm có thể. 

3. Thêm trình khám phá hiện tại vào`current`. Nếu như`current`ít nhất phải bằng yêu cầu của nhà thám hiểm này, hãy thành lập một nhóm và tăng câu trả lời. 

4. Đặt lại`current`về 0 sau khi thành lập nhóm, vì những người khám phá đó đã được sử dụng và không thể đóng góp cho nhóm khác. 

Quyết định quan trọng là đóng cửa một nhóm ngay khi nó có hiệu lực. Giả sử trình thám hiểm hiện tại có yêu cầu`x`và chúng tôi đã có`x`nhà thám hiểm trong nhóm. Việc giữ lại những người khám phá bổ sung cho nhóm này không thể tạo thêm nhóm, trong khi việc hoàn tất hiện tại sẽ mang lại cho những người khám phá còn lại cơ hội tốt nhất để tạo một nhóm hợp lệ khác. 

Tại sao nó hoạt động: 

Sau khi xử lý bất kỳ tiền tố nào của mảng được sắp xếp, thuật toán đã tạo ra số nhóm tối đa có thể chỉ sử dụng tiền tố đó. Một nhóm hoàn chỉnh là hợp lệ vì nó chỉ được tạo khi kích thước của nó ít nhất bằng yêu cầu của mọi trình khám phá được xử lý bên trong nó. Khi thuật toán đóng một nhóm, việc sử dụng bất kỳ người khám phá nào trong nhóm trong tương lai sẽ chỉ loại bỏ những người có sẵn khỏi những người khám phá sau mà không làm tăng số lượng nhóm, vì vậy việc kết thúc nhóm ngay lập tức luôn là điều tối ưu. Việc xử lý các trình khám phá theo thứ tự được sắp xếp đảm bảo rằng mọi yêu cầu được kiểm tra đều là yêu cầu lớn nhất trong nhóm hiện tại, đủ để chứng minh tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        e = list(map(int, input().split()))

        e.sort()

        groups = 0
        current = 0

        for value in e:
            current += 1
            if current >= value:
                groups += 1
                current = 0

        ans.append(str(groups))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp các yêu cầu để quá trình quét tuân theo thứ tự được sử dụng trong bằng chứng tham lam. Biến`current`lưu trữ số lượng nhà thám hiểm đã được thu thập cho nhóm chưa hoàn thành. Mỗi nhà thám hiểm tăng số lượng này lên một. 

Sự so sánh`current >= value`là điều kiện chính xác để đóng một nhóm. Bởi vì mảng đã được sắp xếp,`value`là yêu cầu lớn nhất gặp phải trong nhóm hiện tại, do đó, việc đáp ứng yêu cầu này có nghĩa là mọi nhà thám hiểm trong nhóm đó đều hài lòng. 

Đặt lại`current`về 0 là điều cần thiết. Việc quên điều này sẽ cho phép cùng một nhà thám hiểm đóng góp cho nhiều nhóm và tạo ra một câu trả lời không thể. 

Số nguyên Python không tràn cho vấn đề này và tổng kích thước đầu vào đủ nhỏ để sắp xếp tất cả các trường hợp thử nghiệm trong giới hạn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
1 1 1
```Sau khi sắp xếp, mảng không thay đổi. 

| Yêu cầu thám hiểm | Quy mô nhóm hiện tại | Nhóm được thành lập | 
|---|---|---| 
| 1 | 1, nhóm thân thiết | 1 | 
| 1 | 1, nhóm thân thiết | 2 | 
| 1 | 1, nhóm thân thiết | 3 | 

Mọi nhà thám hiểm đều có thể tạo một nhóm một mình vì mọi yêu cầu đều là một. Quá trình tham lam ngay lập tức đóng mỗi nhóm. 

Đối với mẫu thứ hai:```
5
2 3 1 2 2
```Sau khi sắp xếp:```
1 2 2 2 3
```| Yêu cầu thám hiểm | Quy mô nhóm hiện tại | Nhóm được thành lập | 
|---|---|---| 
| 1 | 1, nhóm thân thiết | 1 | 
| 2 | 1 | 1 | 
| 2 | 2, nhóm thân thiết | 2 | 
| 2 | 1 | 2 | 
| 3 | 2 | 2 | 

Hai nhà thám hiểm cuối cùng không thể tạo nhóm khác vì chỉ còn hai nhà thám hiểm, trong khi một trong số họ cần một nhóm có kích thước ba. Dấu vết cho thấy tại sao các nhóm nhỏ nên được hoàn thiện càng sớm càng tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính và tổng số người khám phá bị giới hạn bởi`3 * 10^5`. | 
| Không gian | O(N) | Yêu cầu lưu trữ mảng yêu cầu bộ nhớ, trong khi bản thân thuật toán chỉ sử dụng một vài bộ đếm. | 

Kích thước đầu vào tối đa đủ lớn để việc tìm kiếm hoặc mô phỏng lặp lại sẽ không thành công, nhưng việc sắp xếp theo sau một lần dễ dàng phù hợp với giới hạn hai giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    t = int(data())
    out = []

    for _ in range(t):
        n = int(data())
        arr = list(map(int, data().split()))
        arr.sort()

        groups = 0
        current = 0

        for x in arr:
            current += 1
            if current >= x:
                groups += 1
                current = 0

        out.append(str(groups))

    sys.stdin = old_stdin
    return "\n".join(out)

assert solution("""2
3
1 1 1
5
2 3 1 2 2
""") == """3
2""", "provided samples"

assert solution("""1
1
1
""") == "1", "single explorer"

assert solution("""1
4
1 1 1 1
""") == "4", "all minimum requirements"

assert solution("""1
5
2 2 2 2 2
""") == "2", "equal requirements"

assert solution("""1
6
1 2 3 4 5 6
""") == "2", "increasing boundary requirements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1 / 1 / 1`|`1`| Kích thước đầu vào tối thiểu | 
|`4 explorers with requirement 1`|`4`| Mỗi nhà thám hiểm tạo thành một nhóm riêng lẻ | 
| Năm nhà thám hiểm có yêu cầu`2`|`2`| Xử lý các giá trị bằng nhau lặp đi lặp lại | 
| Yêu cầu`1 2 3 4 5 6`|`2`| Các trường hợp ranh giới có yêu cầu lớn | 

## Vỏ cạnh 

Đối với trường hợp tất cả các nhà thám hiểm đều có yêu cầu một:```
1
4
1 1 1 1
```Mảng được sắp xếp vẫn giữ nguyên. Bộ đếm đạt đến một sau mỗi trình khám phá, vì vậy thuật toán sẽ đóng một nhóm bốn lần và trả về bốn lần. Một chiến lược ưu tiên các nhóm lớn nhất có thể sẽ trả về một nhóm không chính xác. 

Đối với trường hợp yêu cầu hỗn hợp:```
1
5
2 3 1 2 2
```Sắp xếp mang lại:```
1 2 2 2 3
```Người thám hiểm đầu tiên tạo một nhóm một mình. Hai nhà thám hiểm tiếp theo tạo ra một nhóm có quy mô hai. Hai nhà thám hiểm còn lại không thể đáp ứng yêu cầu thứ ba nên bị bỏ qua. Câu trả lời là hai, phù hợp với số nhóm tối đa có thể. 

Đối với các yêu cầu lớn không thể thực hiện được:```
1
3
1 1 5
```Mảng được sắp xếp là:```
1 1 5
```Hai nhà thám hiểm đầu tiên mỗi người tạo một nhóm có quy mô một. Trình thám hiểm cuối cùng tăng bộ đếm lên một, nhưng yêu cầu là năm nên không có nhóm nào được tạo. Thuật toán trả về hai, là tối ưu vì trình thám hiểm thứ ba không thể thuộc bất kỳ nhóm hợp lệ nào. 
:::
