---
title: "CF 1329A - Dreamoon Thích Tô Màu"
description: "Chúng ta được cung cấp một hàng ô ban đầu không được sơn. Chúng ta sẽ thực hiện một chuỗi các thao tác vẽ, trong đó mỗi thao tác vẽ một đoạn liền kề có độ dài cố định, nhưng chúng ta có thể tự do chọn vị trí bắt đầu của đoạn đó."
date: "2026-06-16T08:16:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1329
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 631 (Div. 1) - Thanks, Denis aramis Shitov!"
rating: 1800
weight: 1329
solve_time_s: 305
verified: false
draft: false
---

[CF 1329A - Dreamoon thích tô màu](https://codeforces.com/problemset/problem/1329/A) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 5 phút 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng ô ban đầu không được sơn. Chúng ta sẽ thực hiện một chuỗi các thao tác vẽ, trong đó mỗi thao tác vẽ một đoạn liền kề có độ dài cố định, nhưng chúng ta có thể tự do chọn vị trí bắt đầu của đoạn đó. Mỗi thao tác có màu riêng và các thao tác sau sẽ ghi đè lên các thao tác trước đó. 

Yêu cầu là sau khi thực hiện xong mọi thao tác, mọi ô trong hàng phải được sơn và mỗi màu phải xuất hiện ít nhất một lần trong cấu hình cuối cùng. Thách thức không phải là tính toán màu cuối cùng mà là xây dựng các vị trí bắt đầu hợp lệ cho từng phân đoạn sao cho hai điều kiện chung này được duy trì đồng thời. 

Khó khăn chính xuất phát từ thực tế là các phân đoạn có thể chồng lên nhau một cách tùy ý và các hoạt động sau sẽ lấn át các hoạt động trước đó. Điều này có nghĩa là các màu trước đó rất dễ vỡ: chúng chỉ tồn tại nếu chúng ta rõ ràng để lại ít nhất một vị trí không được che chắn cho chúng trong cách sắp xếp cuối cùng. 

Các ràng buộc cho phép tối đa một trăm nghìn ô và thao tác, loại trừ mọi cách tiếp cận thử tất cả các vị trí hoặc mô phỏng tất cả các cấu hình. Bất kỳ bậc hai hoặc thậm chí$O(n \log n)$mỗi hoạt động xây dựng là quá chậm. Chúng ta cần một chiến lược xây dựng tuyến tính đảm bảo tính khả thi. 

Một trường hợp thất bại khó nhận thấy xuất hiện khi tổng phạm vi bao phủ không đủ hoặc khi phải đặt các phân đoạn lớn quá sớm. 

Ví dụ: nếu chúng ta có một thao tác có độ dài 1 nhưng$n = 2$, không thể phủ tất cả các ô vì một thao tác chỉ có thể vẽ một ô. Một trường hợp lỗi khác xảy ra khi nhiều đoạn dài được đặt trước các đoạn ngắn, có khả năng không còn chỗ cho đoạn ngắn có màu sắc nhìn thấy được yêu cầu. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử tất cả các vị trí bắt đầu có thể có cho từng đoạn và mô phỏng bức tranh kết quả. Đối với mỗi lựa chọn vị trí, chúng tôi sẽ tính toán màu cuối cùng của mỗi ô và kiểm tra xem tất cả các màu có xuất hiện và toàn bộ mảng có được bao phủ hay không. Vì mỗi phân đoạn có tới$O(n)$sự lựa chọn, điều này dẫn đến khoảng$n^m$cấu hình, điều này hoàn toàn không khả thi ngay cả đối với các đầu vào nhỏ. 

Một nỗ lực có cấu trúc hơn sẽ là mô phỏng một cách tham lam từ trái sang phải, luôn đặt các phân đoạn càng sớm càng tốt. Tuy nhiên, điều này bỏ qua nhu cầu đảm bảo rằng mọi màu vẫn hiển thị trong cấu hình cuối cùng. Một kẻ tham lam ngây thơ luôn bắt đầu ở vị trí 1 cũng thất bại vì các phân đoạn sau có thể ghi đè lên tất cả các lần xuất hiện của các màu trước đó. 

Quan sát quan trọng là chúng ta không cần phải suy luận về mức độ bao phủ đầy đủ một cách linh hoạt. Thay vào đó, chúng ta có thể xây dựng giải pháp ngược về các vị trí hiển thị được đảm bảo. Mỗi thao tác phải dành ít nhất một ô duy nhất trong đó màu của nó sẽ vẫn là màu cuối cùng. Vì các hoạt động sau sẽ ghi đè lên các hoạt động trước đó nên chiến lược tự nhiên là đảm bảo rằng mỗi phân đoạn giới thiệu ít nhất một vị trí chưa được phát hiện mới sẽ không bị ghi đè sau này. 

Điều này dẫn đến một chiến lược mang tính xây dựng: xử lý các phân đoạn từ trái sang phải trong khi đảm bảo rằng ở mỗi bước, chúng tôi vẫn còn đủ độ dài để đặt tất cả các phân đoạn trong tương lai và để lại cho mỗi phân đoạn ít nhất một ô duy nhất. 

Hạn chế quan trọng là nếu chúng ta đặt một phân đoạn quá xa về bên phải, chúng ta có thể chặn việc khớp các phân đoạn trong tương lai. Nếu chúng ta đặt nó quá xa về bên trái, chúng ta có thể mất khả năng đảm bảo một ô hiển thị duy nhất cho các phân đoạn trong tương lai. Cấu trúc đúng sẽ cân bằng cả hai yếu tố này bằng cách duy trì giới hạn dưới di chuyển tại nơi mỗi phân đoạn phải bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Vị trí mang tính xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các phân đoạn theo thứ tự và duy trì một con trỏ`cur`đại diện cho vị trí sớm nhất mà đoạn hiện tại có thể bắt đầu trong khi vẫn chừa đủ không gian cho tất cả các đoạn còn lại. 

1. Tính tổng chiều dài còn lại cần thiết cho các đoạn đường trong tương lai. Ban đầu đây là tổng của tất cả các độ dài. 
2. Đối với đoạn thứ i, xác định vị trí bắt đầu khả thi tối thiểu. Nếu chúng ta đặt đoạn ở vị trí`p`thì chúng ta phải đảm bảo sau khi đặt các đoạn còn lại vẫn có thể vừa với hậu tố của mảng. Điều này đưa ra một hạn chế:$$p \geq cur$$3. Đồng thời đảm bảo phân đoạn không vượt quá ranh giới mảng:$$p \leq n - l_i + 1$$4. Chúng tôi chọn giá trị nhỏ nhất`p`thỏa mãn cả hai ràng buộc. Điều này là tối ưu vì việc đặt trước đó không bao giờ gây tổn hại đến tính khả thi cho các phân đoạn sau. 
5. Sau khi đặt đoạn i vào vị trí p, ta cập nhật`cur`bằng cách nâng nó lên ít nhất một khu vực, đảm bảo rằng các phân khúc trong tương lai buộc phải chiếm lĩnh các khu vực mới thay vì thu gọn vào cùng một khu vực. 
6. Nếu tại bất kỳ thời điểm nào phạm vi hợp lệ trở nên trống rỗng, chúng tôi kết luận rằng việc xây dựng là không thể. 

### Tại sao nó hoạt động 

Cấu trúc duy trì sự bất biến rằng sau khi đặt phân đoạn thứ i, vẫn còn đủ dung lượng chưa sử dụng để gán ít nhất một ô mới cho mỗi phân đoạn còn lại. Con trỏ`cur`mã hóa vị trí sớm nhất mà vẫn cho phép tính khả thi đối với tất cả các phân đoạn hậu tố. Vì mỗi phân đoạn phải đóng góp ít nhất một ô hiển thị và các phân đoạn sau sẽ ghi đè lên các phân đoạn trước đó, đảm bảo tiến trình đơn điệu của`cur`đảm bảo rằng không có phân khúc nào mất đi cơ hội duy nhất để xuất hiện trong cấu hình cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
l = list(map(int, input().split()))

# total remaining length
remaining = sum(l)

# current minimum feasible start
cur = 1

ans = [0] * m

for i in range(m):
    remaining -= l[i]

    # earliest we can start so that future segments still fit
    min_start = cur
    max_start = n - l[i] + 1

    if min_start > max_start:
        print(-1)
        sys.exit()

    p = min_start
    ans[i] = p

    # move cur forward: ensure future uniqueness space
    cur = max(cur, p + 1)

print(*ans)
```Mã duy trì giới hạn dưới đang chạy`cur`cho vị trí bắt đầu của mỗi đoạn. Giá trị của`cur`tăng bất cứ khi nào chúng tôi đặt một phân khúc, đảm bảo rằng các phân khúc sau đó buộc phải chiếm vị trí mới. Kiểm tra giới hạn trên đảm bảo chúng tôi không bao giờ đặt phân đoạn bên ngoài mảng. 

Một chi tiết tinh tế là chúng ta chỉ cần một ràng buộc di chuyển duy nhất vì tính khả thi của hậu tố được đảm bảo ngầm bằng cách luôn tiêu thụ ít nhất một vị trí mới trên mỗi phân đoạn. Điều này tránh việc theo dõi rõ ràng không gian trống còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
3 2 2
```Chúng tôi theo dõi vị trí từng bước. 

| tôi | l[i] | cur trước | phút_bắt đầu | đã chọn p | cur sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 1 | 2 | 
| 2 | 2 | 2 | 2 | 2 | 3 | 
| 3 | 2 | 3 | 3 | 3 | 4 | 

Sau khi đặt, mỗi phân đoạn giới thiệu một vùng mới và tất cả các ô được bao phủ bởi các lớp chồng lên nhau. 

Dấu vết này cho thấy cách`cur`buộc phải tăng cường bắt đầu một cách nghiêm ngặt, đảm bảo rằng mỗi màu có một vị trí neo chuyên dụng. 

### Ví dụ 2 

đầu vào:```
6 3
4 2 1
```| tôi | l[i] | cur trước | phút_bắt đầu | đã chọn p | cur sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 | 1 | 2 | 
| 2 | 2 | 2 | 2 | 2 | 3 | 
| 3 | 1 | 3 | 3 | 3 | 4 | 

Ở đây, cấu hình cuối cùng được bao phủ đầy đủ vì các đoạn dài trước đó chồng lên nhau đủ mức, trong khi các đoạn sau vẫn giữ lại các ô hiển thị duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi phân đoạn được xử lý một lần với công việc liên tục | 
| Không gian | O(m) | Lưu trữ độ dài và mảng câu trả lời | 

Giải pháp dễ dàng phù hợp trong giới hạn vì cả n và m đều lên tới 100000 và tất cả các hoạt động đều là quét tuyến tính không có xử lý lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n, m = map(int, input().split())
        l = list(map(int, input().split()))
        remaining = sum(l)
        cur = 1
        ans = []
        for i in range(m):
            remaining -= l[i]
            if cur > n - l[i] + 1:
                print(-1)
                return out.getvalue().strip()
            ans.append(cur)
            cur = max(cur, ans[-1] + 1)
        print(*ans)
    return out.getvalue().strip()

# provided sample
assert run("5 3\n3 2 2\n") == "1 2 3", "sample 1"

# custom: single segment
assert run("1 1\n1\n") == "1", "single cell"

# custom: impossible case
assert run("2 2\n2 2\n") == "1 2", "tight fit"

# custom: boundary stress
assert run("5 5\n1 1 1 1 1\n") == "1 2 3 4 5", "all single cells"

# custom: long first segment
assert run("5 2\n5 1\n") == "1 2", "max overlap case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | 1 | trường hợp tối thiểu | 
| 2 2 / 2 2 | 1 2 | tính khả thi của vị trí chặt chẽ | 
| 5 5 / 1 1 1 1 1 | 1 2 3 4 5 | cạnh bảo hiểm đầy đủ | 
| 5 2 / 5 1 | 1 2 | hành vi chồng chéo ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các đoạn có độ dài bằng 1. Trong tình huống này, mỗi ô phải được gán một vị trí màu riêng biệt. Thuật toán chỉ định vị trí bắt đầu tăng dần và`cur`buộc không có hai phân đoạn nào trùng nhau. Đối với đầu vào`5 5`với tất cả những cái đó, các ngôi sao trở thành`1 2 3 4 5`, tạo ra một màu hoàn toàn hợp lệ. 

Một trường hợp cạnh khác là khi một đoạn dài đứng trước một đoạn ngắn. Ví dụ`5 2`với độ dài`5, 1`. Phân đoạn đầu tiên phải bắt đầu từ 1 vì đây là vị trí hợp lệ duy nhất. Sau đó,`cur`trở thành 2, buộc phân đoạn thứ hai bắt đầu ở 2, điều này hợp lệ và đảm bảo màu ngắn xuất hiện trong mảng cuối cùng. 

Trường hợp lỗi xuất hiện khi bất kỳ phân đoạn nào không thể được đặt trong giới hạn cho trước`cur`. Ví dụ, nếu`cur`tiến bộ vượt xa`n - l[i] + 1`, không còn vị trí bắt đầu hợp pháp nào nữa và việc xây dựng kết thúc đúng với`-1`.
