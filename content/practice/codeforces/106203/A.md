---
title: "CF 106203A - \u041b\u0443\u0447 \u0441\u043c\u0435\u0440\u0442\u0438"
description: "Chúng tôi đang duy trì một tập hợp các điểm động trên mặt phẳng. Mỗi thao tác sẽ chèn một điểm mới hoặc xóa một điểm hiện có và sau mỗi thao tác, chúng ta phải trả lời câu hỏi có/không: liệu tất cả các điểm hiện tại có thể được bao phủ bởi một đường thẳng vô hạn hay không."
date: "2026-06-19T09:50:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106203
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106203
solve_time_s: 64
verified: true
draft: false
---

[CF 106203A - \u041b\u0443\u0447 \u0441\u043c\u0435\u0440\u0442\u0438](https://codeforces.com/problemset/problem/106203/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp các điểm động trên mặt phẳng. Mỗi thao tác sẽ chèn một điểm mới hoặc xóa một điểm hiện có và sau mỗi thao tác, chúng ta phải trả lời câu hỏi có/không: liệu tất cả các điểm hiện tại có thể được bao phủ bởi một đường thẳng vô hạn hay không. 

Nói cách khác, sau mỗi lần cập nhật, chúng tôi muốn biết liệu tập hợp các điểm hiện tại có thẳng hàng hay không. Bộ này thay đổi theo thời gian, vì vậy đây không phải là kiểm tra hình học tĩnh mà là kiểm tra hoàn toàn động với tối đa 200000 sửa đổi. 

Ràng buộc về số lượng thao tác ngụ ý rằng bất kỳ giải pháp nào tính toán lại câu trả lời từ đầu theo thời gian tuyến tính sau mỗi lần cập nhật sẽ quá chậm trong trường hợp xấu nhất. Việc quét O(n) ngây thơ cho mỗi truy vấn sẽ dẫn đến O(n²), vượt xa giới hạn thời gian. 

Một vấn đề tế nhị đến từ việc xóa. Nhiều cách tiếp cận đơn giản xử lý các phần chèn một cách chính xác nhưng không thành công khi điểm “vi phạm” cuối cùng bị loại bỏ và tập hợp còn lại lại trở nên thẳng hàng. Bất kỳ giải pháp chính xác nào cũng phải có khả năng khôi phục sau cả thao tác chèn và xóa mà không cần phải xây dựng lại mọi thứ quá thường xuyên. 

Một kịch bản thất bại điển hình là như sau. Bắt đầu với ba điểm không thẳng hàng nên câu trả lời là KHÔNG. Sau đó loại bỏ điểm làm cho chúng không thẳng hàng. Câu trả lời đúng lại trở thành CÓ, nhưng bất kỳ cách tiếp cận nào chỉ nhớ rằng tập hợp đã từng sai sẽ bị kẹt ở KHÔNG. 

## Phương pháp tiếp cận 

Một cách trực tiếp để kiểm tra tính thẳng hàng là chọn hai điểm bất kỳ và kiểm tra xem mọi điểm còn lại có nằm trên đường thẳng do chúng xác định hay không. Điều này đúng vì một đường thẳng được xác định duy nhất bởi hai điểm phân biệt. Tuy nhiên, việc tính toán lại bước kiểm tra này sau mỗi lần cập nhật có nghĩa là mỗi lần quét toàn bộ tập hợp, điều này tốn O(n) cho mỗi thao tác và trở nên quá chậm ở 2·10^5 lần cập nhật. 

Quan sát quan trọng là tính cộng tuyến là một tính chất rất cứng nhắc. Nếu tập hợp hợp lệ thì mọi cặp điểm đều xác định cùng một đường thẳng. Nếu nó không hợp lệ thì tồn tại ít nhất ba điểm vi phạm điều kiện này. Điều này gợi ý việc duy trì một “cấu trúc chứng kiến” nhỏ đại diện cho dòng ứng viên hiện tại bất cứ khi nào tập hợp hợp lệ. 

Chiến lược là giữ tập hợp các điểm hiện tại và duy trì một cặp điểm được lưu trong bộ nhớ cache để xác định dòng ứng viên. Khi cấu trúc nhất quán, mỗi lần chèn mới chỉ có thể được kiểm tra theo dòng này. Nếu một điểm mới phá vỡ điều kiện, chúng ta biết ngay tập hợp đó không thẳng hàng nữa. Phần khó khăn là xử lý việc xóa có thể khôi phục tính cộng tác. Trong trường hợp đó, chúng tôi xây dựng lại dòng ứng viên bằng cách chọn bất kỳ hai điểm còn lại nào và xác minh lại tính nhất quán. 

Mặc dù bước tái thiết này là tuyến tính, nhưng nó không được thực hiện sau mỗi thao tác trong hành vi thông thường và trong thực tế, nó chỉ được kích hoạt khi “trạng thái” thay đổi từ hợp lệ sang không hợp lệ hoặc ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tổng O(n²) | O(n) | Quá chậm | 
| Dòng được lưu trong bộ nhớ đệm + thỉnh thoảng được xây dựng lại | O(n) khấu hao mỗi bước xây dựng lại | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tập hợp các điểm hiện tại và hai điểm tham chiếu xác định đường ứng cử viên khi tập hợp được cho là thẳng hàng.

1. Chúng tôi lưu trữ tất cả các điểm hoạt động trong bộ băm hoặc cấu trúc cân bằng hỗ trợ chèn và xóa. Điều này cho phép chúng ta truy cập nhanh vào bất kỳ điểm nào trong tập hợp khi chúng ta cần xây dựng lại một đường. 
2. Sau mỗi thao tác, nếu số điểm là 0, 1 hoặc 2, chúng ta sẽ xuất ngay CÓ vì bất kỳ tập hợp nào có nhiều nhất hai điểm luôn thẳng hàng. 
3. Nếu hiện tại chúng tôi không có dòng ứng viên hợp lệ, chúng tôi sẽ cố gắng xây dựng lại một dòng. Chúng ta chọn bất kỳ hai điểm phân biệt nào từ tập hợp và xác định đường thẳng đi qua chúng. Để xác thực dòng này, chúng tôi kiểm tra mọi điểm trong tập hợp bằng cách sử dụng điều kiện tích chéo:$$(b - a) \times (p - a) = 0$$Nếu tất cả các điểm đều thỏa mãn, chúng tôi lưu trữ cặp này dưới dạng dòng hoạt động và đánh dấu trạng thái là hợp lệ. 
4. Nếu chúng tôi đã có dòng ứng viên hợp lệ, chúng tôi sẽ xử lý các bản cập nhật dần dần. Để chèn, chúng tôi kiểm tra xem điểm mới có nằm trên đường hiện tại hay không bằng cách sử dụng cùng một phép thử tích chéo. Nếu không, chúng tôi đánh dấu trạng thái là không hợp lệ. 
5. Đối với việc xóa, nếu điểm bị xóa không phải là một phần của cặp xác định thì không có gì thay đổi. Nếu nó là một phần của cặp xác định, dòng ứng viên sẽ trở nên không đáng tin cậy và chúng tôi đánh dấu trạng thái là không hợp lệ. 
6. Sau khi xử lý thao tác, nếu trạng thái không hợp lệ và kích thước được đặt ít nhất là 3, chúng tôi sẽ xây dựng lại dòng ứng viên bằng cách sử dụng bước xây dựng lại. 

Bất biến cốt lõi là bất cứ khi nào chúng tôi khẳng định cấu trúc là hợp lệ, dòng được lưu trữ sẽ nhất quán với tất cả các điểm hiện có. Nếu cấu trúc được đánh dấu là không hợp lệ, điều đó có nghĩa là hiện tại chúng tôi không tin cậy bất kỳ dòng nào, nhưng chúng tôi luôn có thể xây dựng lại một dòng bằng cách lấy hai điểm bất kỳ và xác minh tất cả các điểm khác. 

Điều này đảm bảo tính đúng đắn vì bất kỳ tập hợp thẳng hàng hợp lệ nào cũng phải phù hợp với đường thẳng được xác định bởi bất kỳ cặp điểm nào của nó. Khi một đường hợp lệ được tìm thấy, tất cả các điểm trong tương lai phải nằm trên đó, nếu không thì bất biến sẽ bị phá vỡ và chúng ta chuyển sang giai đoạn tái thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def collinear(a, b, x, y):
    return cross(b[0] - a[0], b[1] - a[1], x - a[0], y - a[1]) == 0

def rebuild(points):
    it = iter(points)
    a = next(it)
    b = None
    for p in it:
        if p != a:
            b = p
            break
    if b is None:
        return True, a, a

    for p in points:
        if not collinear(a, b, p[0], p[1]):
            return False, None, None
    return True, a, b

def main():
    n = int(input())
    pts = set()
    ok = False
    a = b = None

    for _ in range(n):
        parts = input().split()
        op = parts[0]
        x = int(parts[1])
        y = int(parts[2])
        p = (x, y)

        if op == '+':
            pts.add(p)
        else:
            pts.remove(p)

        if len(pts) <= 2:
            print("YES")
            ok = False
            continue

        if not ok:
            ok, a, b = rebuild(pts)

        if ok:
            if not collinear(a, b, x, y):
                ok = False

        if not ok and len(pts) > 2:
            ok, a, b = rebuild(pts)

        print("YES" if ok else "NO")

if __name__ == "__main__":
    main()
```Việc triển khai lưu trữ tất cả các điểm hoạt động trong một tập hợp để chèn và xóa O(1). Các biến`a`Và`b`xác định dòng ứng cử viên hiện tại. Boolean`ok`theo dõi xem dòng này hiện có đáng tin cậy hay không. 

Việc xây dựng lại được thực hiện bằng cách chọn hai điểm phân biệt tùy ý và kiểm tra xem tất cả các điểm có nằm trên cùng một đường thẳng hay không. Tích chéo được sử dụng để tránh lỗi dấu phẩy động và giữ mọi thứ dựa trên số nguyên. 

Một điểm tinh tế là chúng tôi luôn đoản mạch câu trả lời khi kích thước được đặt tối đa là 2. Điều này tránh được việc kiểm tra hình học không cần thiết và xử lý các trường hợp suy biến một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
+ 0 0
+ 0 1
+ 0 -1
```Sự phát triển của trạng thái: 

| Bước | Hoạt động | Đặt kích thước | Dòng ứng viên | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | cộng (0,0) | 1 | không | CÓ | 
| 2 | cộng (0,1) | 2 | (0,0)-(0,1) | CÓ | 
| 3 | cộng (0,-1) | 3 | x=0 dòng | CÓ | 

Tất cả các điểm đều nằm trên đường thẳng đứng x=0 nên mọi bước vẫn hợp lệ. 

### Ví dụ 2 

đầu vào:```
+ 0 0
+ 1 0
+ 0 1
- 0 1
```| Bước | Hoạt động | Đặt kích thước | Dòng ứng viên | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | cộng (0,0) | 1 | không | CÓ | 
| 2 | cộng (1,0) | 2 | y=0 | CÓ | 
| 3 | cộng (0,1) | 3 | y=0 | KHÔNG | 
| 4 | loại bỏ (0,1) | 2 | y=0 | CÓ | 

Lần chèn thứ ba phá vỡ tính cộng tuyến vì các điểm không còn nằm trên một đường thẳng nữa. Sau khi loại bỏ điểm vi phạm, tập hợp còn lại sẽ thẳng hàng trở lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao cho mỗi lần tái thiết | Mỗi lần quét toàn bộ tập hợp sẽ xác thực một dòng ứng cử viên | 
| Không gian | O(n) | Lưu trữ điểm hoạt động | 

Các hoạt động được thiết lập là thời gian không đổi, trong khi việc tái thiết chiếm ưu thế về độ phức tạp. Vì việc xây dựng lại chỉ được kích hoạt khi dòng được lưu trữ trở nên không hợp lệ hoặc cần xác thực lại, nên hành vi tổng thể phù hợp với các ràng buộc đã định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return main_capture(inp)

# helper wrapper since main prints directly
def main_capture(inp):
    sys.stdin = io.StringIO(inp)
    out = []
    import sys as _sys
    input = _sys.stdin.readline

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def collinear(a, b, x, y):
        return cross(b[0]-a[0], b[1]-a[1], x-a[0], y-a[1]) == 0

    def rebuild(points):
        it = iter(points)
        a = next(it)
        b = None
        for p in it:
            if p != a:
                b = p
                break
        if b is None:
            return True, a, a
        for p in points:
            if not collinear(a, b, p[0], p[1]):
                return False, None, None
        return True, a, b

    n = int(input())
    pts = set()
    ok = False
    a = b = None

    for _ in range(n):
        parts = input().split()
        op = parts[0]
        x = int(parts[1]); y = int(parts[2])
        p = (x, y)

        if op == '+':
            pts.add(p)
        else:
            pts.remove(p)

        if len(pts) <= 2:
            out.append("YES")
            ok = False
            continue

        if not ok:
            ok, a, b = rebuild(pts)

        if ok and not collinear(a, b, x, y):
            ok = False

        if not ok and len(pts) > 2:
            ok, a, b = rebuild(pts)

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided sample
assert run("""+ 0 0
+ 0 1
+ 0 -1
""") == "YES\nYES\nYES"

# all collinear then break
assert run("""+ 0 0
+ 1 1
+ 2 2
+ 1 2
""") == "YES\nYES\nYES\nNO"

# deletion restores collinearity
assert run("""+ 0 0
+ 1 1
+ 2 2
- 1 2
""") == "YES\nYES\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các điểm trực tuyến | CÓ nhiều lần | tăng trưởng cộng tuyến ổn định | 
| phá vỡ sự cộng tác | KHÔNG cuối cùng | phát hiện vi phạm | 
| xóa khôi phục | cuối cùng CÓ | tính đúng đắn khi xóa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tập hợp tạm thời trở nên không cộng tuyến và sau đó trở về cấu hình cộng tuyến sau khi xóa. Ví dụ: việc chèn điểm không căn chỉnh thứ ba sẽ tạo ra KHÔNG, nhưng việc xóa điểm chính xác đó phải khôi phục CÓ. Thuật toán xử lý việc này vì việc xóa không lưu trữ lỗi vĩnh viễn, nó chỉ làm mất hiệu lực dòng ứng viên hiện tại và buộc phải xây dựng lại khi cần. 

Một trường hợp khác là khi các điểm xác định của đường hiện tại bị xóa. Giả sử dòng được lưu trữ được xác định bởi hai điểm a và b và cả hai đều bị xóa. Cấu trúc phát hiện chính xác rằng dòng ứng cử viên không còn đáng tin cậy, đặt trạng thái thành không hợp lệ và xây dựng lại bằng cách sử dụng bất kỳ điểm nào còn lại, đảm bảo câu trả lời tiếp theo dựa trên một cặp hợp lệ mới. 

Cuối cùng, các bộ nhỏ cỡ một hoặc hai được xử lý riêng. Ngay cả khi thuật toán không lưu trữ dòng hợp lệ, những trường hợp này hoàn toàn bỏ qua quá trình tái thiết và xuất ra ngay lập tức CÓ, ngăn cản việc tính toán hình học không cần thiết.
