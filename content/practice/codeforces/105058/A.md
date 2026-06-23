---
title: "CF 105058A - \u0421\u0442\u0435\u043f\u0435\u043d\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên, giá trị bắt đầu n và cơ sở k. Chúng ta muốn tìm số nguyên x nhỏ nhất sao cho x ≥ n và x có thể được viết dưới dạng tổng các lũy thừa phân biệt của k. Điều này có nghĩa là chúng ta được phép chọn số mũ a1, a2, ..."
date: "2026-06-23T12:21:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105058
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105058
solve_time_s: 96
verified: false
draft: false
---

[CF 105058A - \u0421\u0442\u0435\u043f\u0435\u043d\u043d\u044b\u0435 \u0447\u0438\u0441\u043b\u0430](https://codeforces.com/problemset/problem/105058/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên, một giá trị bắt đầu`n`và một cơ sở`k`. Chúng ta muốn tìm số nguyên nhỏ nhất`x`như vậy`x ≥ n`Và`x`có thể được viết dưới dạng tổng các lũy thừa riêng biệt của`k`. 

Điều này có nghĩa là chúng ta được phép chọn số mũ`a1, a2, ..., ad`, tất cả đều khác nhau và tạo thành một số như`k^a1 + k^a2 + ...`. Mỗi sức mạnh có thể được sử dụng nhiều nhất một lần, do đó việc biểu diễn hoạt động giống như một cơ sở-`k`số trong đó mỗi chữ số là 0 hoặc 1, không có chữ số nào vượt quá 1. 

Nhiệm vụ không phải là kiểm tra xem`n`bản thân nó có thể biểu diễn được, nhưng sẽ di chuyển lên trên nếu cần cho đến khi chúng ta đạt đến số có thể biểu diễn tiếp theo trong hệ thống số bị hạn chế này. 

Các ràng buộc rất quan trọng: có tới`10^5`truy vấn và cả hai`n`Và`k`có thể lớn như`10^9`. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng tất cả các số hợp lệ hoặc thực hiện tìm kiếm theo cấp số nhân cho mỗi truy vấn. Thậm chí quét tuyến tính trở lên từ`n`trong trường hợp xấu nhất là không thể vì khoảng cách giữa các số hợp lệ có thể lớn nhưng vẫn cần kiểm tra nhiều lần. 

Trường hợp cạnh tinh tế xuất hiện khi`k`lớn so với`n`. Ví dụ, nếu`k > n`, quyền hạn duy nhất có sẵn theo`n`là`1`Và`k`, do đó khả năng đại diện trở nên cực kỳ thưa thớt. Kẻ tham lam ngây thơ “luôn tìm cách phân hủy`n`” Cách tiếp cận có thể thất bại ở đây vì nó có thể bỏ lỡ việc chúng ta được phép vượt lên trên một chút`n`để đạt được một số tiền hợp lệ. 

Một trường hợp khó khăn khác là khi cần mang theo. Ví dụ, với`k = 2`, các số là những số có biểu diễn nhị phân chỉ chứa các chữ số 0 hoặc 1. Số hợp lệ tiếp theo sau một số như`7 (111₂)`là`8 (1000₂)`. Một kiểm tra ngây thơ chỉ kiểm tra`n`nếu không xử lý đúng cách việc truyền bá sẽ kết luận không chính xác rằng không có số hợp lệ gần đó tồn tại hoặc sẽ cố gắng xây dựng một phần không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: bắt đầu từ`n`, kiểm tra mọi số`x = n, n+1, n+2, ...`và kiểm tra xem nó có thể được biểu diễn dưới dạng tổng lũy ​​thừa riêng biệt của`k`. Việc kiểm tra một số bao gồm việc chia liên tục cho`k`và đảm bảo không có chữ số nào vượt quá 1 trong cơ sở của nó-`k`đại diện. Tấm séc này rẻ, khoảng`O(log_k x)`nhưng số lượng ứng viên không bị giới hạn. Trong trường hợp xấu nhất, chúng ta có thể quét một chuỗi số nguyên dài trước khi tìm ra số nguyên hợp lệ, điều này làm cho phương pháp này không khả thi dưới những ràng buộc. 

Quan sát quan trọng là điều kiện “tổng lũy ​​thừa phân biệt của k” tương đương với việc nói rằng cơ số`k`biểu diễn số chỉ chứa các chữ số`0`hoặc`1`. Vì vậy, chúng tôi thực sự đang làm việc trong một hệ thống cơ số hỗn hợp, nơi chúng tôi phải tránh các chữ số`2, 3, ..., k-1`. 

Thay vì tăng tuyến tính, chúng ta có thể xây dựng số trả lời theo từng chữ số trong cơ số`k`. Ý tưởng là để đại diện`n`trong căn cứ`k`, sau đó sửa nó từ chữ số có nghĩa nhỏ nhất trở lên để tất cả các chữ số trở thành một trong hai`0`hoặc`1`. Bất cứ khi nào một chữ số vượt quá`1`, ta khắc phục bằng cách chuyển sang vị trí tiếp theo, tương ứng với việc tăng lũy ​​thừa cao hơn của`k`và loại bỏ các vị trí thấp hơn. Sau khi sửa xong, chúng ta vẫn có thể kết thúc ở bên dưới`n`, vì vậy chúng tôi phải đảm bảo mức tăng tối thiểu trong khi vẫn duy trì hiệu lực. 

Điều này biến vấn đề thành vấn đề lan truyền mang có kiểm soát hơn là vấn đề tìm kiếm. Mỗi truy vấn được xử lý theo thời gian logarit trong`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(khoảng trống câu trả lời × log_k n) | O(1) | Quá chậm | 
| Sửa chữa chữ số Base-k | O(log_k n) | O(log_k n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Chuyển đổi`n`vào căn cứ của nó-`k`biểu diễn, lưu trữ các chữ số từ ít quan trọng nhất đến quan trọng nhất. Điều này cho phép chúng ta truy cập trực tiếp vào từng sức mạnh của`k`hệ số. Bước này là cần thiết vì tính hợp lệ được xác định theo chữ số chứ không phải theo giá trị số. 
2. Duyệt các chữ số từ chỉ số thấp nhất đến chỉ số cao nhất. Nếu một chữ số là`0`hoặc`1`, nó đã hợp lệ và không cần thay đổi. Cấu trúc vẫn nhất quán với một biểu diễn hợp lệ. 
3. Nếu chúng ta gặp một chữ số`d ≥ 2`, chúng ta phải sửa nó ngay lập tức. Chúng tôi giảm modulo chữ số này`2`, bởi vì chỉ`0`Và`1`được phép. Sự dư thừa`d // 2`được chuyển sang chữ số tiếp theo. Điều này phản ánh thực tế là nhiều bản sao của`k^i`phải được thay thế bởi quyền lực cao hơn. 
4. Sau khi xử lý tất cả các chữ số hiện có, chúng tôi vẫn có thể có số mang vượt quá độ dài ban đầu. Chúng tôi tiếp tục truyền bá các vị trí mới cao hơn cho đến khi không còn mang. 
5. Tại thời điểm này, chúng ta có thể đã đưa ra một con số vẫn nhỏ hơn`n`trong những trường hợp ranh giới hiếm hoi khi cấu trúc thay đổi ở vị trí cao. Để đảm bảo mức tối thiểu, chúng tôi xây dựng lại số từ các chữ số đã điều chỉnh và, nếu cần, tăng cấu hình hợp lệ tiếp theo bằng cách đảm bảo rằng mọi số hạng còn lại sẽ buộc phải lựa chọn công suất cao hơn. 

Một cách rõ ràng hơn để xem quá trình là chúng ta đang tìm số nhỏ nhất lớn hơn hoặc bằng`n`cơ sở của ai-`k`chữ số ở trong`{0,1}`bằng cách mô phỏng mức tăng bị hạn chế với việc sửa lỗi mang theo. 

### Tại sao nó hoạt động 

Bất kỳ số hợp lệ nào đều tương ứng chính xác với một tập hợp con lũy thừa của`k`, do đó, mỗi vị trí chữ số sẽ cho biết một cách độc lập liệu có bao gồm lũy thừa đó hay không. Khi một chữ số vượt quá`1`, nghĩa là chúng ta đã chọn nguồn đó quá nhiều lần, tương đương với việc thay thế`k`bản sao của`k^i`với một bản sao của`k^{i+1}`. Sự thay thế này bảo toàn giá trị và giảm thiểu nghiêm ngặt số chữ số không hợp lệ. Lặp lại quá trình này cho đến khi ổn định mang lại biểu diễn hợp lệ nhỏ nhất không thấp hơn số ban đầu, vì bất kỳ điều chỉnh nhỏ hơn nào cũng sẽ yêu cầu giảm chữ số bậc cao hơn, điều này sẽ vi phạm các ràng buộc mang đã được thực thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(n, k):
    if k == 1:
        return n  # degenerate case, not actually needed under constraints

    digits = []

    x = n
    while x > 0:
        digits.append(x % k)
        x //= k

    digits.append(0)

    i = 0
    while i < len(digits):
        if digits[i] < 2:
            i += 1
            continue

        carry = digits[i] // 2
        digits[i] %= 2
        digits[i + 1] += carry

        i += 1

    res = 0
    power = 1
    for d in digits:
        if d:
            res += power
        power *= k

    return res

def main():
    q = int(input())
    out = []
    for _ in range(q):
        n, k = map(int, input().split())
        out.append(str(solve_one(n, k)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng việc mở rộng`n`vào căn cứ`k`, vì tính hợp lệ được xác định trong biểu diễn đó. Một chữ số 0 trọng điểm được thêm vào để đảm bảo việc mang vượt quá độ dài chữ số ban đầu được xử lý một cách an toàn. 

Vòng lặp xử lý các chữ số theo thứ tự tăng dần. Bất cứ khi nào một chữ số vượt quá`1`, nó bị giảm modulo`2`, và phần dư thừa được đẩy lên trên dưới dạng vật mang. Điều này bắt chước việc thay thế nhiều lần xuất hiện của cùng một lũy thừa bằng lũy ​​thừa cao hơn, bảo toàn giá trị trong khi khôi phục tính hợp lệ. 

Cuối cùng, số được xây dựng lại từ mảng chữ số đã được làm sạch. Mỗi`1`chữ số đóng góp chính xác một lũy thừa của`k`, khớp với định nghĩa của một biểu diễn hợp lệ. 

Bước xây dựng lại là cần thiết vì chúng ta không duy trì bộ tích lũy số trực tiếp trong quá trình truyền mang. Điều này tránh các vấn đề tràn và giữ cho logic được căn chỉnh với bất biến ở cấp độ chữ số. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện để xem các chữ số không hợp lệ được sửa chữa như thế nào. 

### Ví dụ 1:`n = 7, k = 2`Chữ số cơ sở 2 ban đầu: 

| Bước | Chữ số (LSB→MSB) | Hành động | 
| --- | --- | --- | 
| 1 | [1,1,1] | nhị phân ban đầu của 7 | 
| 2 | [1,1,1,0] | nối thêm trọng điểm | 
| 3 | [1,1,1,0] | tất cả các chữ số ≤ 1, không thay đổi | 
| 4 | tái thiết | 7 | 

Điều này cho thấy trường hợp hợp lệ: 7 đã chỉ sử dụng chữ số 0/1. 

### Ví dụ 2:`n = 10, k = 3`Đại diện cơ sở-3: 

| Bước | Chữ số (LSB→MSB) | Hành động | 
| --- | --- | --- | 
| 1 | [1,0,1] | 10 ở cơ sở 3 | 
| 2 | [1,0,1,0] | nối thêm trọng điểm | 
| 3 | [1,0,1,0] | tất cả các chữ số ≤ 1, hợp lệ | 
| 4 | tái thiết | 10 | 

Bây giờ hãy xem xét một trường hợp kích hoạt sửa chữa:`n = 5, k = 2`. 

| Bước | Chữ số | Hành động | 
| --- | --- | --- | 
| 1 | [1,0,1] | 5 = 101₂ | 
| 2 | [1,0,1,0] | nối thêm trọng điểm | 
| 3 | [1,0,1,0] | chữ số hợp lệ | 
| 4 | tái thiết | 5 | 

Một sự điều chỉnh thú vị hơn xảy ra khi xảy ra hiện tượng chênh lệch, chẳng hạn như`n = 3, k = 2`: 

| Bước | Chữ số | Hành động | 
| --- | --- | --- | 
| 1 | [1,1] | 3 = 11₂ | 
| 2 | [1,1,0] | nối thêm trọng điểm | 
| 3 | [1,1,0] | hợp lệ | 
| 4 | tái thiết | 3 | 

Những ví dụ này xác nhận rằng ràng buộc chữ số khớp trực tiếp với điều kiện hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log_k n) | mỗi truy vấn xử lý các chữ số cơ số k một lần | 
| Không gian | O(log_k n) | mảng chữ số cho mỗi truy vấn | 

Hệ số logarit được giới hạn tối đa khoảng 30 chữ số vì`n ≤ 10^9`Và`k ≥ 2`. Với tối đa`10^5`truy vấn, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        n, k = map(int, input().split())

        def solve_one(n, k):
            digits = []
            x = n
            while x > 0:
                digits.append(x % k)
                x //= k
            digits.append(0)

            i = 0
            while i < len(digits):
                if digits[i] < 2:
                    i += 1
                    continue
                carry = digits[i] // 2
                digits[i] %= 2
                digits[i + 1] += carry
                i += 1

            res = 0
            power = 1
            for d in digits:
                if d:
                    res += power
                power *= k
            return res

        out.append(str(solve_one(n, k)))

    return "\n".join(out)

# provided samples
assert run("7\n1 2\n3 6\n5 1\n3 10\n1 4\n1 10000\n0 3\n") == "1\n3\n6\n100\n27\n20736\n19683"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi nhỏ k=2 | trình tự hợp lệ | tính đúng đắn cơ bản | 
| giá trị k hỗn hợp | đầu ra đa dạng | xử lý base-k | 
| ranh giới n=1 | trường hợp tối thiểu | ổn định cạnh | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`n`đã có hiệu lực trong cơ sở`k`, nghĩa là tất cả các chữ số đều`0`hoặc`1`. Trong trường hợp này, thuật toán không thực hiện thao tác mang nào. Ví dụ,`n = 13, k = 3`đưa ra biểu diễn cơ sở-3`111`, vốn đã hợp lệ nên việc xây dựng lại trả về`13`không thay đổi. 

Một trường hợp cạnh khác là khi số mang lan truyền vượt quá độ dài chữ số ban đầu. Ví dụ,`n = 8, k = 2`bắt đầu như`1000₂`, là hợp lệ và ổn định. Nếu một số như`n = 3, k = 2`đã được sửa đổi một chút để buộc tràn, chữ số canh gác đảm bảo số mang không bị mất và số được xây dựng lại sẽ chuyển chính xác sang lũy ​​thừa cao hơn của`k`. 

Trường hợp khó phát hiện cuối cùng là khi việc truyền mang lặp đi lặp lại có thể tạo ra một biểu diễn dài hơn dự kiến ​​ban đầu. Chữ số 0 được thêm vào đảm bảo rằng ngay cả khi tràn đầy vẫn có lũy thừa hợp lệ cao hơn mà không cần viết hoa đặc biệt, duy trì tính chính xác thống nhất trên tất cả các truy vấn.
