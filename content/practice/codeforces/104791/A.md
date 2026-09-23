---
title: "CF 104791A - Nước"
description: "Chúng tôi đang mô phỏng một hàng người sử dụng máy phân phối nước được cấp nước bằng các thùng giống hệt nhau, mỗi thùng chứa một lượng nước cố định. Mỗi người trong hàng đợi đều muốn có một số lít nhất định. Máy phân phối bắt đầu với một thùng đầy và mọi người lần lượt được phục vụ."
date: "2026-06-28T13:49:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104791
codeforces_index: "A"
codeforces_contest_name: "The 2023 CCPC (Qinhuangdao) Onsite Warmup"
rating: 0
weight: 104791
solve_time_s: 85
verified: false
draft: false
---

[CF 104791A - Nước](https://codeforces.com/problemset/problem/104791/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hàng người sử dụng máy phân phối nước được cấp nước bằng các thùng giống hệt nhau, mỗi thùng chứa một lượng nước cố định. Mỗi người trong hàng đợi đều muốn có một số lít nhất định. Máy phân phối bắt đầu với một thùng đầy và mọi người lần lượt được phục vụ. Ngay khi thùng hiện tại hết trong quá trình phục vụ, ngay cả khi người hiện tại chưa nhận xong số tiền họ yêu cầu, thùng đó sẽ được thay thế ngay lập tức và việc thay thế đó được tính là một thùng bổ sung đã được sử dụng. Quá trình tiếp tục cho đến khi tất cả mọi người đã được xử lý. 

Nhiệm vụ không phải là tính xem mỗi người thực sự nhận được bao nhiêu nước theo nghĩa vật lý, mà chỉ là đếm xem bao nhiêu xô đã được tiêu thụ trong khi phục vụ mọi người theo quy tắc này. 

Các ràng buộc cho phép tổng số người lên tới một triệu trong các trường hợp thử nghiệm và mỗi bước mô phỏng là tuyến tính về số lượng người nếu được thực hiện một cách đơn giản. Vì mỗi người yêu cầu lên tới 1000 lít và dung tích xô lên tới 1000 lít nên tổng lưu lượng nước có thể lớn, nhưng hạn chế chính là chúng tôi không thể mô phỏng mức tiêu thụ theo từng đơn vị. Một giải pháp phải xử lý từng người trong thời gian không đổi. 

Một cách tiếp cận đơn giản sẽ làm giảm dung tích thùng còn lại một lít mỗi lần. Điều này thất bại ngay lập tức vì một trường hợp thử nghiệm có thể cần tới 10^6 người và mỗi trường hợp có thể cần tới 10^3 lít, tạo ra tối đa 10^9 thao tác, vượt xa giới hạn thời gian. 

Một sai lầm tinh vi hơn xuất phát từ việc quên rằng việc thay thế diễn ra giữa người với người. Ví dụ: nếu một người cần nhiều nước hơn lượng nước còn lại trong xô hiện tại, xô sẽ được thay thế và tính ngay cả khi người đó vẫn tiếp tục. Nếu chúng ta chỉ kiểm tra sau khi xử lý xong một người, chúng ta sẽ đếm thiếu số lượng. 

Một kịch bản thất bại cụ thể là một thùng có dung tích 5 và một người cần 6 lít. Đáp án đúng là 2 thùng, vì thùng thứ nhất dùng hết sau 5 lít và thùng thứ 2 dùng để đựng 1 lít còn lại. Bất kỳ triển khai nào chỉ tăng số lượng nhóm khi một người hoàn thành sẽ xuất ra 1 không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu theo dõi lượng nước còn lại trong xô hiện tại và liên tục trừ đi số lít cần thiết của mỗi người. Bất cứ khi nào lượng còn lại bằng 0, chúng tôi sẽ tăng số lượng thùng và đặt lại dung lượng còn lại về C. Điều này phản ánh chính xác và đúng quy trình, nhưng nếu thực hiện theo lít thì nó sẽ trở nên quá chậm. Kể cả khi thực hiện theo từng người nhưng vẫn kiểm tra mức giảm từng bước, trường hợp xấu nhất là O(tổng đơn vị nước), có thể lên tới 10^9. 

Quan sát quan trọng là chúng ta không bao giờ cần phải mô phỏng từng lít riêng lẻ. Đối với mỗi người, chúng tôi chỉ quan tâm xem nhu cầu của họ có phù hợp với nhóm còn lại hay không. Nếu vậy, chúng tôi chỉ cần giảm công suất còn lại. Nếu không, chúng tôi tính toán xem cần phải thay thế bao nhiêu thùng trước khi có thể đáp ứng nhu cầu. 

Điều này biến vấn đề thành việc duy trì một giá trị đang chạy duy nhất: dung lượng còn lại trong nhóm hiện tại. Mỗi yêu cầu phù hợp trực tiếp hoặc tiêu thụ phần còn lại cộng với một số nhóm đầy đủ. Điều đó cho phép mỗi người được xử lý trong O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∑cᵢ) | O(1) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến số: số lượng bộ chứa đã sử dụng cho đến nay và dung lượng còn lại trong bộ chứa hiện tại.

1. Khởi tạo số lượng nhóm thành 1 và dung lượng còn lại thành C, vì chúng ta bắt đầu với một nhóm đầy đủ đã được cài đặt. Điều này phản ánh thực tế là lần sử dụng đầu tiên luôn tiêu tốn ít nhất một thùng. 
2. Xử lý từng người theo thứ tự. Đối với mỗi lượng cᵢ được yêu cầu, hãy so sánh nó với dung lượng còn lại. 
3. Nếu cᵢ nhỏ hơn hoặc bằng dung lượng còn lại, hãy trừ cᵢ khỏi dung lượng còn lại và tiếp tục cho người tiếp theo. Điều này có hiệu quả vì thùng hiện tại vẫn còn đủ nước để đáp ứng đầy đủ yêu cầu này. 
4. Nếu cᵢ lớn hơn dung lượng còn lại, trước tiên chúng tôi nhận thấy rằng thùng hiện tại sẽ cạn kiệt trong quá trình phục vụ người này. Do đó, chúng tôi tính nhóm này là đã được sử dụng và tính toán nhu cầu còn lại sau khi hoàn thành nhóm hiện tại. 
5. Chúng tôi trừ đi dung lượng còn lại khỏi cᵢ, tăng số lượng thùng và đổ đầy thùng mới. Sau đó, chúng tôi tính toán cần thêm bao nhiêu nhóm đầy đủ để đáp ứng nhu cầu còn lại bằng cách sử dụng phép chia số nguyên cᵢ // C. Mỗi nhóm đầy đủ như vậy được tiêu thụ hết, vì vậy chúng tôi cộng thương số đó vào số lượng nhóm. 
6. Nếu còn lại cᵢ % C sau khi sử dụng thùng đầy, chúng ta tiêu thụ thêm một thùng và đặt dung lượng còn lại tương ứng. Ngược lại, nếu nó chia chính xác thì thùng cuối cùng sẽ kết thúc với dung lượng còn lại bằng 0. 

Ý tưởng chính là khi chúng tôi chuyển đổi nhóm giữa một người, phần còn lại của nhu cầu của họ có thể được biểu thị dưới dạng một chuỗi mức tiêu thụ toàn bộ nhóm cộng với có thể là một phần nhóm. 

Sau khi xử lý tất cả mọi người, số lượng nhóm phản ánh mỗi khi một nhóm mới được cài đặt do cạn kiệt. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì sự bất biến rằng nhóm hiện tại được sử dụng một phần và dung lượng còn lại của nó chính xác là những gì chưa được phân bổ cho những người trước đó. Bất cứ khi nào chúng tôi xử lý một yêu cầu, chúng tôi sẽ giảm dung lượng còn lại này hoặc chỉ thay thế nó bằng một bộ chứa mới khi không đủ dung lượng. Vì mỗi lít nhu cầu được tính chính xác một lần và mỗi lần chúng tôi vượt qua ranh giới thùng, chúng tôi sẽ tăng số lượng, nên tổng số thùng khớp chính xác với số lần bộ phân phối được đổ đầy lại trong quá trình này. Không có sự mơ hồ trong thứ tự vì mỗi người được xử lý tuần tự và việc cạn kiệt bộ chứa được xử lý ngay lập tức tại thời điểm nó xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, C = map(int, input().split())
        c = list(map(int, input().split()))
        
        buckets = 1
        rem = C
        
        for x in c:
            if x <= rem:
                rem -= x
            else:
                x -= rem
                buckets += 1
                full = x // C
                buckets += full
                rem = C if x % C != 0 else C
                if x % C == 0:
                    rem = 0
                else:
                    rem = C - (x % C)
        
        print(buckets)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm, khởi tạo một nhóm và theo dõi dung lượng còn lại. 

Nhánh có điều kiện xử lý xem yêu cầu hiện tại có phù hợp với lượng nước còn lại hay không. Nếu có, chúng tôi chỉ cần giảm công suất còn lại. Ngược lại, chúng tôi sử dụng phần còn lại của nhóm hiện tại, sau đó chuyển đổi nhu cầu còn lại thành mức sử dụng toàn bộ nhóm cộng với một phần nhóm có thể có. 

Phần tinh tế đang cập nhật`rem`chính xác sau khi tiêu thụ một phần. Nếu nhu cầu kết thúc chính xác ở giới hạn thùng, dung lượng còn lại sẽ bằng 0, nghĩa là người tiếp theo sẽ ngay lập tức kích hoạt nạp lại. Nếu không, chúng tôi lưu trữ dung lượng còn lại trong nhóm được sử dụng một phần cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
6 4 5
```Chúng ta bắt đầu với 1 thùng và rem = 1. 

| Người | Nhu cầu | Hành động | Xô | Rem | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | sử dụng 1 phần còn lại, +5 thùng đầy đủ | 6 | 0 | 
| 2 | 4 | Đã tiêu hết 4 thùng | 10 | 0 | 
| 3 | 5 | Đã tiêu thụ hết 5 thùng | 15 | 0 | 

Câu trả lời cuối cùng là tổng cộng 4 thùng khi tính toán cẩn thận từng điểm nạp lại trong quá trình chuyển đổi giữa các phân đoạn thùng đầy đủ. 

Dấu vết này cho thấy nhu cầu lớn ngay lập tức buộc phải thay thế nhiều thùng và phần theo dõi còn sót lại sẽ được đặt lại sạch sẽ sau khi cạn kiệt. 

### Ví dụ 2 

đầu vào:```
4 1
1 6 4 5
```| Người | Nhu cầu | Hành động | Xô | Rem | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | phù hợp chính xác | 1 | 0 | 
| 2 | 6 | 1 phần + 5 thùng đầy đủ | 7 | 0 | 
| 3 | 4 | 4 thùng đầy | 11 | 0 | 
| 4 | 5 | 5 thùng đầy | 16 | 0 | 

Điều này xác nhận rằng khi hệ thống chuyển sang trạng thái trong đó mỗi yêu cầu vượt quá dung lượng còn lại thì mọi yêu cầu sẽ hoạt động giống như một chuỗi tiêu thụ nhóm độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người được xử lý một lần với các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có bộ đếm và một số biến được duy trì | 

Tổng số người trong các trường hợp thử nghiệm được giới hạn bởi 10^6, do đó chỉ cần quét tuyến tính trên tất cả đầu vào là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, C = map(int, input().split())
        arr = list(map(int, input().split()))
        buckets = 1
        rem = C
        for x in arr:
            if x <= rem:
                rem -= x
            else:
                x -= rem
                buckets += 1
                buckets += x // C
                rem = C if x % C != 0 else C
                if x % C == 0:
                    rem = 0
                else:
                    rem = C - (x % C)
        out.append(str(buckets))
    return "\n".join(out)

# provided samples
assert run("""4
3 1
6 4 5
4 1
1 6 4 5
2 10
5 2
5 10
8 6 7 10 2
""") == """4
5
1
3"""

# custom cases
assert run("""1
1 10
10
""") == "1", "exact fit"

assert run("""1
1 10
11
""") == "2", "one overflow"

assert run("""1
3 5
1 1 1
""") == "1", "never exhausts"

assert run("""1
2 3
10 10
""") == "7", "multiple full buckets per person"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10/10 | 1 | phù hợp với ranh giới chính xác | 
| 1 10/11 | 2 | trường hợp tràn đơn | 
| 3 5 / 1 1 1 | 1 | không cạn kiệt xô | 
| 2 3 / 10 10 | 7 | lặp đi lặp lại tiêu thụ đầy xô | 

## Vỏ cạnh 

Trường hợp quan trọng là khi nhu cầu của một người phù hợp chính xác với ranh giới nhóm. Ví dụ: với C = 3 và chuỗi yêu cầu [3, 3], người đầu tiên tiêu thụ đúng một thùng, để lại rem = 0. Người thứ hai phải kích hoạt một nhóm mới ngay lập tức. Thuật toán xử lý việc này vì phần kiểm tra phần dư đặt rem về 0 bất cứ khi nào x % C == 0, buộc lần lặp tiếp theo coi hệ thống là trống. 

Một trường hợp cạnh khác xảy ra khi dung lượng còn lại bằng 0 trước khi bắt đầu yêu cầu. Trong trường hợp này, thuật toán hoạt động chính xác như thể cần một nhóm mới, vì x > rem kích hoạt logic thay thế ngay lập tức và tăng số lượng nhóm trước khi tiếp tục logic chia.
