---
title: "CF 105310D - Phạm vi lật"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau và chúng ta muốn chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng các phép toán phạm vi."
date: "2026-06-23T14:58:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105310
codeforces_index: "D"
codeforces_contest_name: "CerealCodes III Advanced Division"
rating: 0
weight: 105310
solve_time_s: 94
verified: false
draft: false
---

[CF 105310D - Lật phạm vi](https://codeforces.com/problemset/problem/105310/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau và chúng ta muốn chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng các phép toán phạm vi. Một thao tác đơn lẻ sẽ chọn một phân đoạn liền kề và áp dụng một trong hai phép biến đổi cố định cho mọi ký tự trong phân đoạn đó: mỗi chữ cái được ánh xạ tới “gương” của nó xung quanh bảng chữ cái (a trở thành z, b trở thành y, v.v.) hoặc mỗi chữ cái được xoay 13 vị trí trong bảng chữ cái (a trở thành n, b trở thành o, v.v., bao quanh). 

Điểm mấu chốt là mỗi thao tác áp dụng thống nhất trong một khoảng thời gian và chúng tôi muốn số lượng tối thiểu các thao tác khoảng thời gian đó làm cho toàn bộ chuỗi khớp chính xác với mục tiêu hoặc xác định rằng không thể thực hiện được. 

Ràng buộc n lên tới 10^6 ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các khoảng hoặc tìm kiếm theo trạng thái. Bất cứ điều gì bậc hai trong n là không thể. Ngay cả O(n log n) cũng chỉ được chấp nhận nếu nó là một lần chuyển hoặc sử dụng các cấu trúc dữ liệu đơn giản. Cấu trúc gợi ý rõ ràng rằng chúng ta nên suy luận cục bộ cho mỗi vị trí và sau đó hợp nhất các quyết định giữa các phân khúc. 

Trường hợp cạnh tinh tế xuất hiện khi một ký tự trong s không thể đạt tới ký tự tương ứng của nó trong t bằng cách sử dụng bất kỳ sự kết hợp nào của hai phép biến đổi được phép. Ví dụ: nếu s = "a" và t = "b", thì việc đảo ngược hay dịch chuyển 13 không tạo ra sự khớp một bước và việc áp dụng các thao tác trong bất kỳ khoảng thời gian nào cũng không thể giúp ích vì mỗi vị trí tiến hóa độc lập theo sự kết hợp của hai phép tiến hóa này. Điều này làm cho câu trả lời ngay lập tức là -1 trong những trường hợp như vậy. 

Một tình huống phức tạp khác là khi các vị trí liền kề yêu cầu các phép biến đổi khác nhau. Ví dụ: nếu một chỉ mục cần một thao tác ngược và chỉ mục tiếp theo cần một thao tác ngược lại, thì việc nhóm ngây thơ có thể đếm thừa hoặc đếm thiếu các hoạt động tùy thuộc vào cách hợp nhất các khoảng thời gian. Giải pháp đúng đắn phải nhận ra rằng về cơ bản chúng ta đang hình thành các phân đoạn có “sự khác biệt về yêu cầu vận hành” nhất quán. 

## Phương pháp tiếp cận 

Mỗi vị trí i xác định một phép biến đổi cần thiết để ánh xạ s[i] tới t[i], nếu có thể. Chỉ có ba trạng thái có ý nghĩa cho một vị trí: không cần thay đổi, áp dụng ngược lại hoặc áp dụng ngược lại. Quan sát đầu tiên là cả hai phép biến đổi đều là các phép biến đổi và cấu trúc thành phần của chúng nhỏ và cố định. Nếu chúng ta biểu thị đảo ngược là R và ngược lại là O, thì việc áp dụng một trong hai trên một phân đoạn có nghĩa là chúng ta chuyển đổi trạng thái trên khoảng đó. 

Brute Force sẽ cố gắng quyết định áp dụng R hay O cho mỗi khoảng thời gian, tìm kiếm hiệu quả hơn 2 lựa chọn cho mỗi khoảng thời gian và nhiều kết hợp theo cấp số nhân. Ngay cả việc cố gắng chọn các phân đoạn một cách tham lam cũng không thành công vì việc áp dụng một thao tác sẽ thay đổi tất cả các ký tự trong phân đoạn, do đó các lựa chọn cục bộ có thể lan truyền một cách khó lường. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các chữ cái và thay vào đó hãy nghĩ về sự khác biệt giữa s và t. Đối với mỗi vị trí, chúng tôi xác định xem nó cần lật R, lật O, cả hai hay không. Điều này làm giảm vấn đề trong việc theo dõi cách các yêu cầu này thay đổi dọc theo chuỗi. 

Một vị trí cần chuyển đổi cụ thể có thể được coi là nhãn. Vì các thao tác áp dụng cho các phân đoạn liền kề nên vấn đề trở thành việc đếm số lần lật phân đoạn tối thiểu cần thiết để nhận ra nhãn mục tiêu, trong đó mỗi lần lật chuyển đổi một trong hai lớp nhị phân độc lập. 

Điều này rút gọn thành một quan sát cổ điển: câu trả lời là số khối liền kề trong đó trạng thái hoạt động được yêu cầu thay đổi, riêng biệt cho từng loại hoạt động, sau khi xác thực tính nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trong khoảng thời gian | Hàm mũ | O(n) | Quá chậm | 
| Quét chênh lệch tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi dịch từng ánh xạ ký tự sang trạng thái hoạt động cần thiết.

1. Với mỗi chỉ số i, hãy tính xem phép biến đổi nào ánh xạ s[i] thành t[i]. Chúng tôi kiểm tra xem nó là đồng nhất, đảo ngược hay ngược lại. Nếu không khớp, chúng tôi ngay lập tức trả về -1. Bước này là cần thiết vì các thao tác chỉ tạo ra một tập hợp hoán vị khép kín nhỏ, do đó những sai lệch không thể sửa chữa sau này. 
2. Chúng tôi biểu thị mỗi vị trí dưới dạng một cặp cờ nhị phân cho biết liệu có cần đảo ngược hay không và có cần đảo ngược hay không. Điều này có hiệu quả vì việc áp dụng đảo ngược hai lần sẽ bị loại bỏ và điều tương tự cũng xảy ra đối với trường hợp ngược lại. 
3. Chúng tôi quét chuỗi và xử lý từng cờ độc lập dưới dạng chuỗi 0/1 trên các vị trí. 
4. Với mỗi chuỗi, hãy đếm xem giá trị thay đổi bao nhiêu lần giữa các chỉ số liền kề. Mỗi thay đổi cho biết sự bắt đầu của một phân đoạn mới mà chúng ta phải áp dụng thao tác đó. Điều này là tối ưu vì một lần lật khoảng thời gian có thể bao gồm bất kỳ khối liền kề tối đa nào có yêu cầu giống hệt nhau. 
5. Tổng số đoạn cần thiết cho đảo ngược và ngược lại một cách độc lập. Tổng này là số lần di chuyển tối thiểu. 

### Tại sao nó hoạt động 

Mỗi thao tác tương ứng với việc lật một khoảng liền kề trong một mảng nhị phân. Bất kỳ giải pháp nào cũng có thể được phân tách thành các vùng liền kề tối đa mà cần phải lật. Trong mỗi khu vực, một thao tác là đủ và việc chia một khu vực sẽ chỉ làm tăng số lần di chuyển. Vì đảo ngược và ngược lại hoạt động độc lập trên các ràng buộc nhị phân riêng biệt, nên câu trả lời tổng thể là tổng của các phân đoạn tối ưu cho mỗi ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def transform_type(a, b):
    x = ord(a) - ord('a')
    y = ord(b) - ord('a')

    if x == y:
        return (0, 0)

    # reverse: x + y == 25
    if x + y == 25:
        return (1, 0)

    # opposite: shift by 13
    if (x + 13) % 26 == y:
        return (0, 1)

    return None

def solve():
    n = int(input())
    s = input().strip()
    t = input().strip()

    rev = []
    opp = []

    for i in range(n):
        res = transform_type(s[i], t[i])
        if res is None:
            print(-1)
            return
        r, o = res
        rev.append(r)
        opp.append(o)

    def count_segments(arr):
        if n == 0:
            return 0
        cnt = arr[0]
        for i in range(1, n):
            if arr[i] != arr[i - 1]:
                cnt += arr[i]
        return cnt

    print(count_segments(rev) + count_segments(opp))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên giảm mỗi phép biến đổi ký tự thành một cặp yêu cầu boolean. Chức năng trợ giúp`transform_type`mã hóa xem một vị trí cần đảo ngược hay ngược lại và từ chối các ánh xạ không thể thực hiện được. 

Sau đó, chúng tôi duy trì hai mảng nhị phân, một mảng dành cho các yêu cầu ngược lại và một mảng dành cho các yêu cầu ngược lại. chức năng`count_segments`tính toán có bao nhiêu nhóm số 1 liền kề xuất hiện, vì mỗi nhóm tương ứng với chính xác một phép toán khoảng. 

Một chi tiết tinh tế là chúng ta chỉ tăng khi gặp sự chuyển đổi từ 0 sang 1; chúng tôi dựa vào thực tế là việc bắt đầu một phân đoạn tương ứng chính xác với khoảng thời gian hoạt động bắt buộc mới. Chúng tôi không tính các chuyển đổi từ 1 đến 0 vì những chuyển đổi đó chỉ đánh dấu sự kết thúc chứ không phải chuyển động mới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

s = "abcde", t = "abcde" 

Cả hai mảng đều giống hệt nhau nên không cần thực hiện thao tác nào. 

| tôi | s[i] | t[i] | vòng quay | đối | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | 0 | 0 | 
| 1 | b | b | 0 | 0 | 
| 2 | c | c | 0 | 0 | 
| 3 | d | d | 0 | 0 | 
| 4 | e | e | 0 | 0 | 

Cả hai mảng đều là các số 0 không đổi, vì vậy số lượng phân đoạn là 0. Câu trả lời là 0. 

Điều này xác nhận trường hợp cơ bản không cần chuyển đổi. 

### Ví dụ 2 

đầu vào: 

s = "aaa", t = "znmnm" 

Chúng tôi tính toán các hoạt động cần thiết cho mỗi ký tự. 

| tôi | s[i] | t[i] | vòng quay | đối | 
| --- | --- | --- | --- | --- | 
| 0 | một | z | 1 | 0 | 
| 1 | một | n | 0 | 1 | 
| 2 | một | m | 0 | 1 | 
| 3 | một | n | 0 | 1 | 
| 4 | một | m | 0 | 1 | 

Mảng đảo ngược: [1,0,0,0,0] cho 1 đoạn. 

Mảng đối diện: [0,1,1,1,1] cho 1 đoạn. 

Tổng số câu trả lời là 2. 

Điều này cho thấy các yêu cầu chạy độc lập chuyển trực tiếp thành các hoạt động theo khoảng thời gian như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để phân loại ký tự cộng với một lượt để đếm phân đoạn | 
| Không gian | O(n) | Lưu trữ hai mảng nhị phân có độ dài n | 

Giải pháp là tuyến tính theo độ dài chuỗi, vừa vặn thoải mái trong các giới hạn lên tới 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def transform_type(a, b):
        x = ord(a) - ord('a')
        y = ord(b) - ord('a')
        if x == y:
            return (0, 0)
        if x + y == 25:
            return (1, 0)
        if (x + 13) % 26 == y:
            return (0, 1)
        return None

    n = int(sys.stdin.readline())
    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    rev = []
    opp = []

    for i in range(n):
        res = transform_type(s[i], t[i])
        if res is None:
            return "-1"
        r, o = res
        rev.append(r)
        opp.append(o)

    def count_segments(arr):
        if n == 0:
            return 0
        cnt = arr[0]
        for i in range(1, n):
            if arr[i] != arr[i - 1]:
                cnt += arr[i]
        return cnt

    return str(count_segments(rev) + count_segments(opp))

assert run("5\nabcde\nabcde\n") == "0"
assert run("5\naaaaa\nznmnm\n") == "2"
assert run("1\na\nb\n") == "-1"
assert run("3\nabc\nzyx\n") == "1"
assert run("6\naaaaaa\nnnnnnn\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | 0 | không cần thao tác | 
| ánh xạ xen kẽ hỗn hợp | 2 | đếm phân đoạn độc lập | 
| nhân vật duy nhất không thể | -1 | từ chối tính khả thi | 
| đoạn đảo ngược đầy đủ | 1 | hoạt động liền kề duy nhất | 
| toàn bộ đoạn đối diện | 1 | hoạt động liền kề duy nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các ký tự không yêu cầu chuyển đổi. Thuật toán tạo ra cả hai mảng chứa đầy số 0, do đó không có phần bắt đầu nào được tính và đầu ra bằng 0. Điều này giúp tránh tính sai số lần chuyển tiếp khi bắt đầu. 

Một trường hợp khác là chuyển đổi hoàn toàn đồng nhất như đảo ngược mọi ký tự. Mảng đảo ngược trở thành tất cả một và bộ đếm phân đoạn nhìn thấy chính xác một khối liền kề. Điều này xác nhận rằng hoạt động toàn dải là tối ưu thay vì chia thành các khoảng nhỏ hơn. 

Trường hợp thứ ba là các yêu cầu xen kẽ, chẳng hạn như 101010. Ở đây, mỗi thay đổi đều đưa ra một khởi đầu phân đoạn mới, do đó, mỗi yêu cầu riêng biệt sẽ trở thành hoạt động riêng của nó. Thuật toán nắm bắt được điều này một cách tự nhiên vì mỗi lần chuyển đổi từ 0 đến 1 sẽ tăng số lượng.
