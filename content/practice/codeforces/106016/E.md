---
title: "CF 106016E - a nhỏ hơn b"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau và chúng ta được phép sửa đổi chuỗi đầu tiên bằng nhiều nhất một thao tác. Thao tác đó chọn một phân đoạn liền kề và đảo ngược nó."
date: "2026-06-21T16:42:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106016
codeforces_index: "E"
codeforces_contest_name: "The 2025 Homs Collegiate programming contest"
rating: 0
weight: 106016
solve_time_s: 66
verified: true
draft: false
---

[CF 106016E - a nhỏ hơn b](https://codeforces.com/problemset/problem/106016/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau và chúng ta được phép sửa đổi chuỗi đầu tiên bằng nhiều nhất một thao tác. Thao tác đó chọn một phân đoạn liền kề và đảo ngược nó. Sau khi không làm gì hoặc thực hiện chính xác một lần đảo ngược như vậy, chúng ta so sánh chuỗi kết quả với chuỗi thứ hai theo thứ tự từ điển. Nhiệm vụ là xác định xem liệu chúng ta có thể làm cho chuỗi đầu tiên nhỏ hơn chuỗi thứ hai hay không. 

So sánh từ điển hoạt động giống như thứ tự từ điển: chúng tôi quét từ trái sang phải và vị trí đầu tiên nơi các ký tự khác nhau sẽ quyết định kết quả. 

Ràng buộc rằng tổng của tất cả độ dài chuỗi tối đa là 100000 sẽ loại trừ ngay lập tức mọi giải pháp thử tất cả các khoảng thời gian đảo ngược có thể có. Có các khoảng O(n2) và thậm chí kiểm tra tuyến tính cho mỗi khoảng thời gian cũng đã vượt quá giới hạn. Điều này đẩy chúng ta tới một quy trình quyết định tham lam hoặc dựa trên tiền tố trong đó mỗi trường hợp kiểm thử được xử lý theo thời gian tuyến tính. 

Một điểm tinh tế là thao tác này có thể làm xấu đi các phần của chuỗi. Việc đảo ngược một đoạn không “cải thiện cục bộ”; nó sắp xếp lại một khối, do đó, hiệu ứng phải được suy luận trên toàn cầu thông qua thứ tự từ điển, không chỉ thông qua so sánh cục bộ. 

Một trường hợp quan trọng là khi các chuỗi đã bằng nhau. Thậm chí khi đó, sự đảo ngược có thể làm cho chuỗi đầu tiên nhỏ hơn. Ví dụ: nếu chúng ta có một chuỗi như`ba`, đảo ngược mang lại`ab`, nhỏ hơn`ba`mặc dù chúng tôi bắt đầu bằng mục tiêu theo nghĩa giả định. Một giải pháp ngây thơ chỉ kiểm tra các trường hợp đã tồn tại sự không khớp đầu tiên sẽ không thành công ở đây. 

Một trường hợp khác là khi không sử dụng đảo chiều. Câu trả lời ngay lập tức là có nếu chuỗi gốc đã nhỏ hơn về mặt từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: thử từng cặp chỉ số l và r, đảo ngược phân đoạn đó và so sánh chuỗi kết quả với chuỗi đích. Mỗi lần đảo ngược có chi phí O(n) và có các lựa chọn phân đoạn O(n2), do đó độ phức tạp tổng thể sẽ trở thành O(n³) cho mỗi trường hợp thử nghiệm. Ngay cả việc giảm chi phí so sánh xuống O(1) bằng cách sử dụng hàm băm luân phiên cũng không khắc phục được số lượng ứng viên cơ bản O(n²), do đó, việc này vẫn quá chậm. 

Quan sát quan trọng là thứ tự từ điển được xác định bởi vị trí đầu tiên mà chúng ta có thể buộc phải cải tiến. Khi chúng tôi sửa vị trí i, mọi thứ trước i phải giữ nguyên với chuỗi gốc (vì bất kỳ thay đổi nào trước đó sẽ chiếm ưu thế so sánh). Vì vậy, sự đảo ngược, nếu có ích, nên được chọn để tác động đến vị trí sớm nhất có thể cần cải thiện. 

Điều này làm giảm vấn đề khi quyết định, đối với mỗi vị trí tiền tố i, liệu chúng ta có thể đưa một ký tự nhỏ hơn ký tự tương ứng trong b[i] vào vị trí i bằng cách sử dụng một đảo ngược mà không ảnh hưởng đến các ký tự trước đó hay không. 

Bên trong đoạn đảo ngược, cách duy nhất để đưa một ký tự về phía trước là chọn nó làm điểm cuối phù hợp của đoạn. Vì vậy, đối với mỗi vị trí i, chúng ta muốn biết liệu có tồn tại một số j ≥ i sao cho việc đảo ngược a[i..j] đặt a[j] ở vị trí i và ký tự đó đủ nhỏ để cải thiện thứ tự từ điển hay không. 

Điều này dẫn đến việc duy trì, đối với mỗi hậu tố, ký tự nhỏ nhất có sẵn, bởi vì đó là ứng cử viên tốt nhất mà chúng tôi có thể tiến tới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Trước tiên hãy kiểm tra xem chuỗi gốc a đã nhỏ hơn về mặt từ điển so với chuỗi b hay chưa. Nếu có, chúng ta có thể trả lời ngay là có vì thao tác này là tùy chọn. 
2. Tính toán trước một cấu trúc cho phép chúng ta biết, với mỗi vị trí i, ký tự nhỏ nhất trong a[i..n−1] là gì. Chúng tôi cũng theo dõi một chỉ mục nơi ký tự nhỏ nhất này xuất hiện, tốt nhất là xuất hiện ở ngoài cùng bên phải để chúng tôi có sự linh hoạt tối đa trong việc hình thành phân đoạn đảo chiều. 
3. Quét vị trí từ trái sang phải. Tại vị trí i, chúng ta giả sử tất cả các vị trí trước đó khớp chính xác với b, vì nếu không thì việc so sánh từ điển sẽ được quyết định sớm hơn. 
4. Tại mỗi vị trí i, chúng ta so sánh ký tự tốt nhất có thể mà chúng ta có thể mang đến vị trí i, đây là ký tự tối thiểu trong hậu tố bắt đầu từ i, với b[i]. 
5. Nếu ký tự hậu tố tối thiểu này nhỏ hơn b[i] và chỉ số xuất hiện j của nó ít nhất là i thì chúng ta có thể chọn l = i và r = j. Việc đảo ngược đoạn này sẽ đưa a[j] đến vị trí i trong khi không chạm tới các vị trí trước đó. Điều này đảm bảo rằng sự không khớp đầu tiên với b xảy ra tại i và thuận lợi, vì vậy chúng ta có thể trả lời ngay là có. 
6. Nếu ký tự hậu tố tối thiểu bằng b[i] thì ngay cả nước đi tốt nhất có thể cũng không thể cải thiện vị trí i, vì vậy chúng ta tiếp tục đến chỉ mục tiếp theo. 
7. Nếu quét xong mà không tìm được vị trí i hợp lệ thì câu trả lời là không. 

Tính đúng đắn phụ thuộc vào thực tế là việc cải thiện từ điển phải xảy ra ở vị trí sớm nhất mà sự khác biệt có thể được thực thi. Bất kỳ nỗ lực nào nhằm bỏ qua chỉ mục có thể cải thiện đầu tiên và sửa chỉ mục sau sẽ bị chi phối bởi các ràng buộc bình đẳng trước đó, vì các vị trí trước đó vẫn khớp với b và ngăn cản lợi thế sau này trở nên quan trọng. 

### Tại sao nó hoạt động

Tại bất kỳ chỉ số i nào, nếu chúng ta quyết định rằng các vị trí từ 0 đến i−1 phải bằng b thì cách duy nhất để thắng là buộc a[i] < b[i]. Thao tác đảo ngược chỉ có thể đưa một ký tự từ hậu tố vào vị trí i và ký tự tốt nhất là ký tự tối thiểu của hậu tố đó. Nếu ngay cả ký tự đó không nhỏ hơn b[i] thì không có sự đảo ngược nào bắt đầu từ hoặc sau i có thể cải thiện thứ tự từ điển ở vị trí i và việc trì hoãn quyết định chỉ khiến vấn đề trở nên tồi tệ hơn vì các vị trí trước đó vẫn cố định. Điều này tạo ra một cấu trúc quyết định đơn điệu trong đó cải tiến khả thi đầu tiên luôn là cải tiến tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        a = input().strip()
        b = input().strip()

        # If already smaller, no need to use operation
        if a < b:
            print("Yes")
            continue

        # suffix minimum character and position
        n = len(a)
        suf_min = [''] * n
        suf_pos = [0] * n

        suf_min[-1] = a[-1]
        suf_pos[-1] = n - 1

        for i in range(n - 2, -1, -1):
            if a[i] < suf_min[i + 1]:
                suf_min[i] = a[i]
                suf_pos[i] = i
            else:
                suf_min[i] = suf_min[i + 1]
                suf_pos[i] = suf_pos[i + 1]

        ok = False

        for i in range(n):
            if suf_min[i] < b[i]:
                j = suf_pos[i]
                if j >= i:
                    ok = True
                    break
            if a[i] != b[i]:
                break

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ kiểm tra trường hợp tầm thường trong đó không cần thực hiện thao tác nào. Sau đó, nó xây dựng hậu tố cực tiểu để đối với mỗi vị trí, chúng ta có thể biết ngay nhân vật tốt nhất có thể được kéo về phía trước bằng một sự đảo ngược bắt đầu từ đó. Quá trình quét buộc chúng tôi chỉ xem xét các vị trí mà tiền tố vẫn khớp với chuỗi đích, vì nếu không thì việc so sánh từ điển sẽ bị phân kỳ. 

Chi tiết triển khai quan trọng là lưu trữ cả ký tự tối thiểu và chỉ mục nơi nó xuất hiện. Nếu không có chỉ mục, chúng ta sẽ biết ký tự nào cần đưa lên phía trước nhưng không biết liệu nó có thực sự được chuyển đến vị trí thứ i bằng cách sử dụng một đảo ngược hợp lệ hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

a = "bca" 

b = "xe taxi" 

Chúng tôi tính toán hậu tố tối thiểu: 

| tôi | một [tôi] | hậu tố tối thiểu | vị trí | 
| --- | --- | --- | --- | 
| 0 | b | một | 2 | 
| 1 | c | một | 2 | 
| 2 | một | một | 2 | 

Tại i = 0, hậu tố min là`a`, Và`a < b[0] = c`, vì vậy chúng ta có thể đảo ngược [0,2], tạo ra`"acb"`, nhỏ hơn`"cab"`. 

Dấu vết này cho thấy một sự đảo ngược đơn lẻ có thể kéo ký tự nhỏ nhất trên toàn cầu về phía điểm quyết định sớm nhất như thế nào. 

### Ví dụ 2 

đầu vào: 

a = "abc" 

b = "abc" 

Hậu tố tối thiểu: 

| tôi | một [tôi] | hậu tố tối thiểu | vị trí | 
| --- | --- | --- | --- | 
| 0 | một | một | 0 | 
| 1 | b | b | 1 | 
| 2 | c | c | 2 | 

Không có vị trí nào có hậu tố tối thiểu nhỏ hơn b[i]. Không có sự đảo ngược nào có thể tạo ra sự cải thiện nghiêm ngặt ở bất kỳ vị trí nào mà không phá vỡ sự bình đẳng trước đó, vì vậy câu trả lời là Không. 

Điều này chứng tỏ rằng ngay cả khi các chuỗi bằng nhau, cấu trúc vẫn ngăn chặn chính xác các kết quả dương tính giả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được xử lý bằng một lần truyền hậu tố và một lần quét | 
| Không gian | O(n) | Lưu trữ cho hậu tố tối thiểu và vị trí | 

Tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 100000, do đó, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input().strip())
            a = input().strip()
            b = input().strip()

            if a < b:
                print("Yes")
                continue

            n = len(a)
            suf_min = [''] * n
            suf_pos = [0] * n

            suf_min[-1] = a[-1]
            suf_pos[-1] = n - 1

            for i in range(n - 2, -1, -1):
                if a[i] < suf_min[i + 1]:
                    suf_min[i] = a[i]
                    suf_pos[i] = i
                else:
                    suf_min[i] = suf_min[i + 1]
                    suf_pos[i] = suf_pos[i + 1]

            ok = False
            for i in range(n):
                if suf_min[i] < b[i]:
                    ok = True
                    break
                if a[i] != b[i]:
                    break

            print("Yes" if ok else "No")

    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-style checks (illustrative)
assert run("1\n2\nza\nza\n") == "Yes"
assert run("1\n3\nabb\nabb\n") == "No"
assert run("1\n4\nabza\naaza\n") == "No"

# custom cases
assert run("1\n3\nabc\nabc\n") == "No"
assert run("1\n3\ncba\nabc\n") == "Yes"
assert run("1\n1\na\nb\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi nhỏ bằng nhau | Không | không thể cải thiện sự đảo ngược | 
| trường hợp có lợi ngược lại | Có | hậu tố tối thiểu cho phép cải tiến | 
| ký tự đơn | Có | cải thiện từ điển tầm thường | 
| chuỗi đã tối ưu | Không | ngăn ngừa dương tính giả | 

## Vỏ cạnh 

Khi các chuỗi đã bằng nhau, thuật toán vẫn cố gắng tìm một cách chính xác vị trí mà ký tự hậu tố có thể được di chuyển về phía trước để tạo ra mức giảm nghiêm ngặt. Nếu không có vị trí như vậy tồn tại thì quá trình quét sẽ không bao giờ thành công và câu trả lời vẫn là Không. Ví dụ: với`a = "abc"`Và`b = "abc"`, mọi hậu tố tối thiểu đều khớp với ký tự tương ứng, do đó, không sự đảo ngược nào có thể tạo ra lợi thế nghiêm ngặt ở bất kỳ vị trí nào. 

Khi ký tự đầu tiên đã nhỏ hơn, chẳng hạn như`a = "a..."`Và`b = "b..."`, thuật toán phát hiện ngay thành công tại i = 0 mà không cần xét đến cấu trúc sâu hơn. Hậu tố tối thiểu ở vị trí 0 nhiều nhất là`a[0]`, tức là đã đủ thỏa mãn điều kiện. 

Khi cải tiến chỉ tồn tại sâu trong chuỗi, quá trình quét sẽ đảm bảo các vị trí trước đó chặn cải tiến đó một cách chính xác. Nếu xảy ra sự không khớp trước bất kỳ cải tiến khả thi nào, vòng lặp sẽ sớm bị ngắt, duy trì tính chính xác bằng cách tôn trọng mức độ ưu tiên từ điển.
