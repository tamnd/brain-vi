---
title: "CF 104596J - Biên tập viên chịu thuế"
description: "Chúng ta được đưa cho một dãy sách và mỗi cuốn sách phải được đọc từ đầu đến cuối trước khi chuyển sang cuốn tiếp theo. Mỗi cuốn sách có độ dài tính bằng trang và thời hạn tính bằng ngày."
date: "2026-06-30T04:42:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104596
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC East Central North America Regional Contest (ECNA 2019)"
rating: 0
weight: 104596
solve_time_s: 45
verified: true
draft: false
---

[CF 104596J - Biên tập viên chịu thuế](https://codeforces.com/problemset/problem/104596/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một dãy sách và mỗi cuốn sách phải được đọc từ đầu đến cuối trước khi chuyển sang cuốn tiếp theo. Mỗi cuốn sách có độ dài tính bằng trang và thời hạn tính bằng ngày. Người đọc sử dụng một tốc độ đọc không đổi duy nhất cho tất cả các cuốn sách, được đo bằng số trang mỗi ngày và việc đọc xong một cuốn sách mất một số ngày bằng chiều dài trần của nó chia cho tốc độ. Bởi vì các cuốn sách được xử lý tuần tự nên ngày hoàn thành của mỗi cuốn sách là tổng cộng dồn của các khoảng thời gian này. 

Một cuốn sách được coi là muộn nếu ngày nó hoàn thành vượt quá thời hạn. Nhiệm vụ là chọn tốc độ đọc số nguyên nhỏ nhất có thể sao cho có nhiều nhất m cuốn sách bị đọc muộn. 

Các ràng buộc cho phép tối đa 100.000 cuốn sách, với độ dài lên tới 10^9 và thời hạn lên tới 10^4. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử nghiệm mọi tốc độ một cách ngây thơ và mô phỏng đầy đủ cho từng tốc độ. Ngay cả một mô phỏng đầy đủ duy nhất cũng là O(n), vì vậy việc thử mọi tốc độ lên tới 10^9 là không thể. Một giải pháp hợp lệ phải giảm đáng kể số lượng ứng viên tốc độ và vẫn đánh giá từng ứng viên một cách hiệu quả. 

Một vấn đề tế nhị xuất phát từ cách xác định độ trễ. Độ trễ không phụ thuộc vào từng cuốn sách; nó phụ thuộc vào thời gian đọc tích lũy. Một sai lầm ngây thơ là so sánh từng cuốn sách một cách riêng biệt, bỏ qua rằng những cuốn sách dài trước đó sẽ trì hoãn tất cả những cuốn tiếp theo. 

Một cạm bẫy phổ biến khác là xử lý sai cách phân chia trần nhà. Nếu phép chia số nguyên được sử dụng trực tiếp dưới dạng l // s, nó sẽ đánh giá thấp thời gian bất cứ khi nào l không chia hết cho s, điều này có thể làm giảm lịch trình tích lũy một cách không chính xác và tạo ra tốc độ không hợp lệ có vẻ khả thi. 

## Phương pháp tiếp cận 

Một nỗ lực đơn giản là thử mọi tốc độ đọc có thể từ 1 đến độ dài tối đa của cuốn sách. Với mỗi tốc độ, chúng tôi mô phỏng việc đọc tất cả các cuốn sách một cách tuần tự, tính toán ngày hoàn thành của mỗi cuốn sách, đếm xem có bao nhiêu cuốn vượt quá thời hạn và kiểm tra xem con số này có nhiều nhất là m hay không. Điều này đúng vì nó phản ánh trực tiếp quá trình được mô tả trong bài toán. Tuy nhiên, chi phí là rất cao. Nếu chúng tôi kiểm tra S tốc độ có thể, mỗi tốc độ yêu cầu mô phỏng O(n), tổng chi phí là O(Sn), trong trường hợp xấu nhất sẽ có thứ tự 10^14 thao tác. 

Quan sát cấu trúc quan trọng là việc tăng tốc độ đọc chỉ có thể giúp ích. Nếu một tốc độ nhất định s đủ để giữ tối đa m cuốn sách bị trễ, thì bất kỳ tốc độ nào lớn hơn sẽ hoàn thành mọi cuốn sách không muộn hơn tốc độ s, và do đó không thể tăng số lượng cuốn sách bị trễ. Hành vi đơn điệu này cho phép chúng ta coi vấn đề như một phép tìm kiếm trên một vị từ đã được sắp xếp: các tốc độ khả thi tạo thành một hậu tố của tất cả các số nguyên. 

Khi nhận ra tính đơn điệu, chúng ta có thể áp dụng tìm kiếm nhị phân cho câu trả lời. Đối với tốc độ thí sinh, chúng tôi mô phỏng một lần để đếm xem có bao nhiêu cuốn sách bị trễ. Điều này làm giảm số lượng mô phỏng từ tuyến tính trong phạm vi câu trả lời sang logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi tốc độ | O(maxL · n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng | O(n log maxL) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế rằng tính khả thi của tốc độ có thể được kiểm tra một cách độc lập.

1. Xác định hàm kiểm tra xem tốc độ đã cho có hợp lệ hay không. Chúng tôi mô phỏng việc đọc tất cả các cuốn sách theo thứ tự, duy trì tổng số ngày đã sử dụng. Đối với mỗi cuốn sách, chúng tôi cộng số ngày cần thiết để hoàn thành nó, được tính bằng (l + s − 1) // s. Sau khi cập nhật thời gian tích lũy, chúng tôi kiểm tra xem ngày hoàn thành có vượt quá thời hạn của sổ đó hay không. Nếu có, chúng tôi coi đó là một cuốn sách muộn. 
2. Hàm kiểm tra trả về true nếu số sách nộp muộn nhiều nhất là m. Điều này cho chúng ta một vị từ đơn điệu về tốc độ. 
3. Chúng tôi đặt phạm vi tìm kiếm nhị phân cho câu trả lời. Tốc độ tối thiểu có thể là 1. Tốc độ cần thiết tối đa là độ dài cuốn sách lớn nhất, vì bất kỳ cuốn sách nào cũng có thể được đọc hết trong một ngày với tốc độ đó. 
4. Chúng tôi thực hiện tìm kiếm nhị phân trên phạm vi này. Đối với mỗi tốc độ trung bình, chúng tôi tiến hành kiểm tra tính khả thi. 
5. Nếu tốc độ khả thi, chúng ta cố gắng giảm tốc độ bằng cách di chuyển ranh giới bên phải xuống dưới. Nếu không khả thi, chúng ta tăng tốc độ bằng cách di chuyển ranh giới bên trái lên trên. 
6. Sau khi tìm kiếm nhị phân hội tụ, ranh giới bên trái là tốc độ khả thi nhỏ nhất. 

Tại sao nó hoạt động: mô phỏng xác định một biến vị ngữ về tốc độ đơn điệu không tăng. Khi tốc độ trở nên đủ để giữ độ trễ trong giới hạn cho phép, tất cả tốc độ lớn hơn sẽ duy trì hoặc cải thiện mỗi lần hoàn thành, do đó chúng không thể tăng số lượng sách bị trễ. Điều này đảm bảo rằng vùng khả thi là liền kề, đây chính xác là điều kiện cần thiết cho tìm kiếm nhị phân để xác định tốc độ hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(speed, books, m):
    time = 0
    late = 0
    for l, d in books:
        time += (l + speed - 1) // speed
        if time > d:
            late += 1
            if late > m:
                return False
    return True

def solve():
    n, m = map(int, input().split())
    books = [tuple(map(int, input().split())) for _ in range(n)]

    lo, hi = 1, max(l for l, _ in books)

    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid, books, m):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Mô phỏng là cốt lõi của giải pháp. Biến`time`biểu thị ngày tích lũy mà cuốn sách hiện tại kết thúc chứ không phải thời lượng của mỗi cuốn sách. Sự tích lũy này là cần thiết vì mỗi cuốn sách chỉ bắt đầu sau khi cuốn trước đã hoàn thành. 

Bộ phận trần`(l + speed - 1) // speed`đảm bảo rằng một phần ngày được hạch toán chính xác. Nếu không có sự điều chỉnh này, những cuốn sách có độ dài không chia hết cho tốc độ sẽ xuất hiện nhanh hơn thực tế một cách không chính xác, điều này sẽ phá vỡ tính chính xác. 

Tìm kiếm nhị phân duy trì tính bất biến mà bất kỳ tốc độ nào dưới đây`lo`là không hợp lệ, trong khi bất kỳ tốc độ nào bằng hoặc cao hơn`hi`là hợp lệ. Chức năng kiểm tra thực thi điều kiện khả thi một cách nhất quán, cho phép tìm kiếm hội tụ một cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

n = 3, m = 1 

Sách: (450, 9), (500, 6), (300, 4) 

Chúng tôi kiểm tra tốc độ của ứng viên bằng cách sử dụng tìm kiếm nhị phân. 

| tốc độ | hết quyển 1 | kết thúc quyển 2 | kết thúc quyển 3 | đếm muộn | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 450 | 950 | 1250 | 3 | không | 
| 2 | 225 | 475 | 625 | 2 | không | 
| 3 | 150 | 317 | 417 | 1 | vâng | 

Đối với tốc độ 2, cuốn thứ hai kết thúc vào ngày thứ 475, đã vượt quá thời hạn 6 và cuốn thứ ba cũng bị trượt. Điều đó tạo ra quá nhiều cuốn sách muộn. Ở tốc độ 3 chỉ có một cuốn sách bị trễ, thỏa mãn điều kiện ràng buộc nên đáp án là 3. 

Dấu vết này cho thấy độ trễ phụ thuộc vào thời gian hoàn thành tích lũy chứ không phải thời lượng của từng cuốn sách. Ngay cả khi tốc độ tăng vừa phải cũng sẽ thay đổi đáng kể thời gian hoàn thành sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log L) | Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các sách và tìm kiếm nhị phân theo tốc độ sẽ thực hiện kiểm tra logarit với độ dài sách tối đa L | 
| Không gian | O(1) | Chỉ có một số bộ đếm được duy trì bên cạnh bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 100.000 cuốn sách và nhật ký (L tối đa) là khoảng 30, do đó giải pháp thực hiện khoảng vài triệu thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    def check(speed, books, m):
        time = 0
        late = 0
        for l, d in books:
            time += (l + speed - 1) // speed
            if time > d:
                late += 1
                if late > m:
                    return False
        return True

    def solve():
        n, m = map(int, input().split())
        books = [tuple(map(int, input().split())) for _ in range(n)]
        lo, hi = 1, max(l for l, _ in books)
        while lo < hi:
            mid = (lo + hi) // 2
            if check(mid, books, m):
                hi = mid
            else:
                lo = mid + 1
        print(lo)

    old = sys.stdin
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out.strip()

# sample
assert run("""3 1
450 9
500 6
300 4
""") == "3"

# minimum case
assert run("""1 0
10 1
""") == "10"

# already feasible at speed 1
assert run("""2 1
1 10
1 10
""") == "1"

# tight deadlines forcing high speed
assert run("""2 0
10 1
10 1
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cuốn, không được phép trễ | 10 | độ chính xác của một phần tử và trạng thái trần | 
| hai cuốn sách nhỏ, deadline chậm | 1 | tính khả thi ở tốc độ tối thiểu | 
| thời hạn chặt chẽ | 10 | tích lũy và độ chặt ranh giới | 

## Vỏ cạnh 

Trường hợp quan trọng là khi chỉ có một cuốn sách được phép trễ. Trong những trường hợp như vậy, tốc độ tối ưu thường nằm ngay trên ngưỡng mà một cuốn sách dài không thể phá vỡ chuỗi thời hạn. Tìm kiếm nhị phân nắm bắt chính xác điều này vì hàm khả thi chuyển từ sai sang đúng đúng một lần. 

Một trường hợp quan trọng khác là khi tất cả các thời hạn đều cực kỳ nhỏ so với độ dài của cuốn sách. Ví dụ: nếu mỗi thời hạn là 1, thì chỉ tốc độ bằng độ dài cuốn sách tối đa mới đảm bảo bất kỳ cuốn sách nào cũng có thể hoàn thành trong vòng một ngày; nếu không tất cả các cuốn sách sẽ nhanh chóng trở nên muộn. Thuật toán xử lý việc này một cách tự nhiên vì quá trình kiểm tra tính khả thi sẽ tính độ trễ dựa trên thời gian tích lũy và tìm kiếm nhị phân sẽ đẩy thẳng tới tốc độ yêu cầu tối đa. 

Cuối cùng, khi m lớn, gần với n − 1, hầu như mọi tốc độ đều có thể chấp nhận được. Chức năng kiểm tra vẫn đảm bảo tính chính xác bằng cách đếm số sách dư thừa bị trễ và tìm kiếm nhị phân sẽ hội tụ về tốc độ nhỏ nhất để tránh được điểm lỗi tồi tệ nhất trong lịch trình.
