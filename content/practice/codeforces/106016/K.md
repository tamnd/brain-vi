---
title: "CF 106016K - Bánh quy"
description: "Một hàng trẻ được đưa ra, mỗi trẻ mang một đánh giá. Chúng ta phải chỉ định một số lượng cookie dương cho mỗi đứa trẻ, nhưng việc chỉ định không thể tùy tiện."
date: "2026-06-22T03:55:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106016
codeforces_index: "K"
codeforces_contest_name: "The 2025 Homs Collegiate programming contest"
rating: 0
weight: 106016
solve_time_s: 65
verified: true
draft: false
---

[CF 106016K - Cookie](https://codeforces.com/problemset/problem/106016/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một hàng trẻ được đưa ra, mỗi trẻ mang một đánh giá. Chúng ta phải chỉ định một số lượng cookie dương cho mỗi đứa trẻ, nhưng việc chỉ định không thể tùy tiện. Yêu cầu về cấu trúc duy nhất mang tính cục bộ: bất cứ khi nào một đứa trẻ có xếp hạng cao hơn hẳn so với đứa trẻ ngay trước chúng, đứa trẻ đó phải nhận được nhiều cookie hơn người hàng xóm bên trái của chúng. 

Không có hạn chế theo hướng ngược lại. Nếu xếp hạng giảm xuống, giữ nguyên hoặc tăng vọt theo bất kỳ cách nào không tăng, thì số lượng cookie có thể làm được bất cứ điều gì miễn là mỗi đứa trẻ đều có ít nhất một chiếc. 

Nhiệm vụ là giảm thiểu tổng số cookie trên tất cả trẻ em đồng thời tôn trọng những so sánh cục bộ này. 

Kích thước đầu vào nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi trường hợp có tối đa 100 trường hợp con. Ngay cả cách tiếp cận bậc hai hoặc bậc ba cũng có thể được chấp nhận, nhưng cấu trúc của điều kiện cho thấy việc xây dựng tuyến tính là có thể. 

Một sai lầm ngây thơ xuất hiện khi mọi người cố gắng thực thi các ràng buộc theo cả hai hướng hoặc cố gắng cân bằng lại tổng thể số lượng sau lần phân công ban đầu. Ví dụ, hãy xem xét xếp hạng`[1, 3, 2]`. Một quy tắc đối xứng bất cẩn có thể cho`[1, 2, 1]`, điều này không thành công vì con thứ hai phải vượt quá con thứ nhất. Một chế độ lỗi khác là cố gắng chỉ điều chỉnh khi phát hiện thấy vi phạm sau khi vượt qua hoàn toàn, điều này có thể xếp tầng các thay đổi ngược liên tục. 

Khó khăn chính là hạn chế chỉ lan truyền từ trái sang phải chứ không lan truyền ngược lại, điều này khiến các quyết định cục bộ trở thành quyết định cuối cùng sau khi được đưa ra. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi số lượng cookie của mỗi đứa trẻ như một biến bị ràng buộc bởi sự bất bình đẳng: mỗi đứa trẻ có ít nhất một cookie và đối với mỗi vị trí mà xếp hạng tăng lên, chúng tôi áp đặt sự bất bình đẳng nghiêm ngặt giữa các biến liền kề. Người ta có thể cố gắng tìm kiếm trên tất cả các phép gán hợp lệ hoặc liên tục nới lỏng các ràng buộc cho đến khi hội tụ. 

Cách tiếp cận như vậy nhanh chóng trở nên không khả thi vì số lượng phân phối cookie có thể tăng theo cấp số nhân với n. Ngay cả khi chúng tôi giới hạn các giá trị trong phạm vi hợp lý, việc sửa lỗi vi phạm nhiều lần có thể yêu cầu nhiều lượt vượt qua và trong trường hợp xấu nhất, mỗi lần điều chỉnh sẽ lan truyền trên toàn bộ mảng nhiều lần, dẫn đến hành vi bậc hai hoặc tệ hơn cho mỗi trường hợp thử nghiệm. 

Cấu trúc của ràng buộc đơn giản hơn lần đầu tiên nó xuất hiện. Mọi hạn chế chỉ so sánh một đứa trẻ với người hàng xóm bên trái ngay lập tức của chúng. Không có khớp nối toàn cầu. Điều này có nghĩa là chúng ta có thể quyết định giá trị hợp lệ tối thiểu cho từng vị trí một cách tham lam khi chúng ta biết giá trị của vị trí trước đó. 

Quan sát quan trọng là lần duy nhất chúng tôi buộc phải tăng cookie là khi xếp hạng tăng nghiêm ngặt. Nếu xếp hạng không tăng, chúng tôi có thể đặt lại về giá trị tối thiểu có thể là 1. Điều này loại bỏ mọi nhu cầu điều chỉnh ngược hoặc tối ưu hóa tổng thể. 

Chúng tôi kết thúc bằng một đường chuyền từ trái sang phải trong đó mỗi vị trí chỉ được xác định bởi vị trí trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Thư giãn ràng buộc | O(n²) theo cấp số nhân | O(n) | Quá chậm | 
| Quét tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo câu trả lời cho đứa trẻ đầu tiên là 1. Điều này là bắt buộc vì mỗi đứa trẻ phải có ít nhất một cookie và không có ràng buộc bên trái nào. 
2. Duyệt mảng từ con thứ hai đến con cuối cùng. Tại mỗi vị trí i, so sánh xếp hạng hiện tại với xếp hạng trước đó. 
3. Nếu xếp hạng hiện tại lớn hơn xếp hạng trước đó, hãy gán cookie[i] = cookie[i−1] + 1. Đây là giá trị nhỏ nhất thỏa mãn bất đẳng thức nghiêm ngặt trong khi vẫn giữ tổng giá trị tối thiểu. Bất kỳ giá trị nhỏ hơn sẽ vi phạm quy tắc. 
4. Ngược lại, gán cookie[i] = 1. Vì không có yêu cầu khi xếp hạng không tăng nên việc chọn giá trị nhỏ nhất có thể sẽ giữ tổng số tiền ở mức tối thiểu. 
5. Tích lũy tổng của tất cả các giá trị được chỉ định khi chúng ta tính toán chúng, thay vì lưu trữ toàn bộ mảng nếu muốn tối ưu hóa bộ nhớ. 

### Tại sao nó hoạt động 

Ở mỗi vị trí, việc phân công chỉ phụ thuộc vào vị trí trước đó và so sánh xếp hạng cục bộ. Sau khi cookie[i−1] được sửa, cookie[i] hợp lệ nhỏ nhất được xác định đầy đủ: 1 hoặc cookie[i−1] + 1 nếu ràng buộc kích hoạt. Bất kỳ nỗ lực nào nhằm giảm cookie[i] hơn nữa sẽ ngay lập tức vi phạm ràng buộc duy nhất liên quan đến chỉ mục i. Vì không có ràng buộc nào trong tương lai có thể buộc phải giảm so với các vị trí trước đó, nên các cực tiểu cục bộ này kết hợp thành một giải pháp tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        r = list(map(int, input().split()))

        cookies = 1
        prev = 1
        total = 1

        for i in range(1, n):
            if r[i] > r[i - 1]:
                prev = prev + 1
            else:
                prev = 1
            total += prev

        out.append(str(total))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại giá trị cookie trước đó thay vì lưu trữ toàn bộ mảng. Điều này là đủ vì mỗi bước chỉ phụ thuộc vào bước trước đó. Biến`prev`đại diện cho các cookie được chỉ định của trẻ hiện tại và nó được cập nhật theo so sánh xếp hạng. 

Sự tích lũy vào`total`tránh việc xây dựng lại mảng ở cuối. Điều này phù hợp trực tiếp với logic tham lam. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
1 2 2 3 2
```Chúng tôi theo dõi nhiệm vụ từng bước. 

| tôi | đánh giá | đánh giá trước | hành động | cookie[i] | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | bắt đầu | 1 | 1 | 
| 1 | 2 | 1 | tăng | 2 | 3 | 
| 2 | 2 | 2 | không lớn hơn | 1 | 4 | 
| 3 | 3 | 2 | tăng | 2 | 6 | 
| 4 | 2 | 3 | không lớn hơn | 1 | 7 | 

Câu trả lời cuối cùng là 7. Dấu vết cho thấy việc đặt lại về 1 xảy ra như thế nào bất cứ khi nào điều kiện tăng bị phá vỡ. 

### Ví dụ 2 

đầu vào:```
1
4
4 3 2 1
```| tôi | đánh giá | đánh giá trước | hành động | cookie[i] | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | - | bắt đầu | 1 | 1 | 
| 1 | 3 | 4 | không lớn hơn | 1 | 2 | 
| 2 | 2 | 3 | không lớn hơn | 1 | 3 | 
| 3 | 1 | 2 | không lớn hơn | 1 | 4 | 

Trường hợp này xác nhận rằng khi dãy giảm nghiêm ngặt, phép gán tối ưu sẽ thu gọn về tất cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đứa trẻ được xử lý một lần trong một lượt từ trái sang phải | 
| Không gian | O(1) thêm | Chỉ một biến đang chạy được duy trì | 

Với n lên tới 100 và t lên tới 100, tổng công việc là không đáng kể ngay cả trong những giới hạn nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    def solve():
        input = sys.stdin.readline
        t = int(input())
        res = []
        for _ in range(t):
            n = int(input())
            r = list(map(int, input().split()))
            prev = 1
            total = 1
            for i in range(1, n):
                if r[i] > r[i - 1]:
                    prev += 1
                else:
                    prev = 1
                total += prev
            res.append(str(total))
        print("\n".join(res))

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("1\n3\n1 2 3\n") == "6"

# single element
assert run("1\n1\n100\n") == "1"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "5"

# decreasing
assert run("1\n4\n9 8 7 6\n") == "4"

# mixed
assert run("1\n5\n1 3 2 2 4\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | n | không bắt buộc phải tăng | 
| dãy giảm dần | n | thiết lập lại hành vi | 
| mẫu hỗn hợp | 7 | độ chính xác lan truyền cục bộ | 

## Vỏ cạnh 

Đối với một đứa trẻ, thuật toán sẽ chỉ định ngay một cookie vì không có cookie trước đó để so sánh. Bộ quy tắc không áp đặt thêm ràng buộc nào nữa nên đầu ra vẫn là 1. 

Đối với một chuỗi tăng nghiêm ngặt như`[1, 2, 3, 4]`, thuật toán tăng liên tục`prev`, sản xuất`[1, 2, 3, 4]`. Điều này xác nhận rằng mức tăng theo chuỗi được lan truyền chính xác mà không cần bất kỳ điều chỉnh ngược nào. 

Đối với một chuỗi giảm nghiêm ngặt như`[5, 4, 3, 2]`, mọi so sánh đều không đạt được điều kiện, vì vậy`prev`được đặt lại về 1 ở mỗi bước. Kết quả trở thành`[1, 1, 1, 1]`, là mức tối thiểu vì không có ràng buộc nào buộc phải tăng lên.
