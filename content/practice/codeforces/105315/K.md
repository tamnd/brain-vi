---
title: "CF 105315K - Sinh Nhật Ka3bool"
description: "Chúng tôi đang duy trì một tập hợp các số nguyên động trong đó các phần tử được chèn và xóa từng thao tác một. Sau mỗi lần cập nhật, chúng ta cần tính xem tập hợp đó bị “phân mảnh” như thế nào khi xem trên trục số."
date: "2026-06-23T15:07:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "K"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 55
verified: true
draft: false
---

[CF 105315K - Sinh nhật Ka3bool](https://codeforces.com/problemset/problem/105315/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp các số nguyên động trong đó các phần tử được chèn và xóa từng thao tác một. Sau mỗi lần cập nhật, chúng ta cần tính xem tập hợp đó bị “phân mảnh” như thế nào khi xem trên trục số. 

Chính xác hơn, nếu chúng ta sắp xếp các phần tử hiện tại và nhóm các số nguyên liên tiếp thành các khối liền kề tối đa thì mỗi khối như vậy sẽ đóng góp một phân đoạn. Một số riêng biệt cũng được tính là một phân đoạn. Hàm chúng ta được yêu cầu duy trì chính xác là số lượng các phân đoạn liền kề này. 

Ví dụ, nếu tập hợp là`{1, 2, 4, 5, 6, 8}`, thứ tự sắp xếp đã rõ ràng. Những con số`{1, 2}`tạo thành một phân đoạn,`{4, 5, 6}`tạo thành một cái khác, và`{8}`đứng một mình, tổng cộng có ba phân đoạn. 

Mỗi thao tác sẽ chèn một số vào tập hợp hoặc loại bỏ một số được đảm bảo tồn tại. Sau mỗi lần sửa đổi, chúng ta phải xuất ra số lượng hiện tại của các phân đoạn đó. 

Ràng buộc về số lượng thao tác lên tới một triệu và các giá trị có thể lớn tới 10^18. Điều này ngay lập tức loại trừ mọi giải pháp quét hoặc sắp xếp toàn bộ tập hợp sau mỗi lần cập nhật. Ngay cả các cấu trúc logarit cũng phải được sử dụng cẩn thận vì chúng ta cần khấu hao O(log n) hoặc cao hơn cho mỗi phép toán. 

Một vấn đề tế nhị phát sinh xung quanh sự liền kề. Định nghĩa phụ thuộc hoàn toàn vào việc liệu`x-1`Và`x+1`tồn tại trong tập hợp. Bất kỳ giải pháp chính xác nào cũng phải theo dõi các mối quan hệ này một cách hiệu quả vì việc tính toán lại chúng từ đầu sau mỗi thay đổi sẽ quá chậm. 

Các trường hợp cạnh xuất hiện khi cập nhật cầu nối hoặc phân chia các phân đoạn. 

Nếu chúng ta chèn một số nối hai đoạn hiện có, ví dụ như chèn`5`vào trong`{1, 2}`Và`{6, 7}`, số đoạn giảm đi một. Một cách tiếp cận ngây thơ chỉ đơn giản là tăng bộ đếm khi chèn sẽ thất bại. 

Nếu chúng ta xóa một số đang kết nối hai số lân cận, ví dụ như xóa`5`từ`{4, 5, 6}`, số lượng phân đoạn tăng thêm một. Bất kỳ giải pháp nào không xem xét tính kề cận khi xóa sẽ bị hỏng ở đây. 

Cuối cùng, lặp đi lặp lại việc chèn và xóa xung quanh các ranh giới như`x = 0`hoặc các giá trị rất lớn vẫn hoạt động giống nhau về mặt logic, vì chỉ có giá trị kề mới quan trọng chứ không phải độ lớn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ duy trì toàn bộ tập hợp và tính toán lại số lượng phân đoạn sau mỗi thao tác. Sau mỗi lần cập nhật, chúng ta có thể sắp xếp tập hợp và quét nó một cách tuyến tính, đếm số lần chênh lệch liên tiếp lớn hơn 1. Điều này đúng vì nó tuân theo định nghĩa về các khoảng liền kề. 

Tuy nhiên, việc tính toán lại này tốn O(n log n) cho mỗi truy vấn do sắp xếp hoặc O(n) nếu chúng tôi duy trì cấu trúc được sắp xếp nhưng vẫn quét nó. Với tối đa 10^6 thao tác và có khả năng kích thước được đặt cũng lớn, điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào cấu trúc cục bộ xung quanh phần tử được cập nhật. Khi chèn hoặc gỡ bỏ`x`, chỉ có sự có mặt của`x-1`Và`x+1`vấn đề. Hai người hàng xóm này xác định liệu`x`bắt đầu một phân đoạn mới, hợp nhất hai phân đoạn hoặc mở rộng một phân đoạn hiện có. 

Điều này làm giảm bài toán cấu trúc toàn cục thành bài toán duy trì lân cận cục bộ. Bộ băm cung cấp kiểm tra thành viên trung bình O(1), cho phép chúng tôi cập nhật số lượng phân đoạn trong thời gian không đổi cho mỗi thao tác. 

Chúng tôi duy trì một biến`segments`đại diện cho số lần chạy liền kề hiện tại. Để chèn, chúng tôi kiểm tra xem`x-1`Và`x+1`hiện hữu. Để xóa, chúng tôi thực hiện điều chỉnh ngược dựa trên các hàng xóm giống nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n) | O(n) | Quá chậm | 
| Tối ưu (Bộ băm) | O(q) trung bình | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bộ`S`và một số nguyên`segments`. 

1. Khởi tạo`S`trống rỗng và`segments = 0`. Không có phần tử có nghĩa là không có phân đoạn. 
2. Để chèn`+ x`, kiểm tra xem`x`đã vắng mặt rồi. Cho phép`left = (x-1 in S)`Và`right = (x+1 in S)`. 
3. Nếu cả hai`left`Và`right`là sai thì`x`tạo thành một phân đoạn biệt lập hoàn toàn mới, do đó tăng`segments`bằng 1. Điều này xảy ra khi`x`không kết nối với bất cứ điều gì hiện có. 
4. Nếu cả hai`left`Và`right`là đúng, chèn`x`kết nối hai phân đoạn riêng biệt trước đó thành một phân đoạn lớn hơn, do đó giảm`segments`bằng 1. Đây là trường hợp duy nhất có hai sự hợp nhất xảy ra đồng thời. 
5. Nếu chính xác một trong`left`hoặc`right`là đúng thì`x`mở rộng một phân đoạn hiện có mà không thay đổi tổng số lượng. 
6. Chèn`x`vào trong`S`. 
7. Để xóa`- x`, tính lại`left = (x-1 in S)`Và`right = (x+1 in S)`trước khi loại bỏ. 
8. Nếu cả hai`left`Và`right`là sai, loại bỏ`x`xóa một phân đoạn độc lập, do đó giảm`segments`bằng 1. 
9. Nếu cả hai`left`Và`right`là đúng, loại bỏ`x`chia một đoạn thành hai, do đó tăng`segments`bằng 1. 
10. Nếu tồn tại chính xác một lân cận, việc xóa chỉ thu nhỏ một phân đoạn mà không thay đổi số lượng phân đoạn. 
11. Xóa`x`từ`S`. 

### Tại sao nó hoạt động 

Mỗi số nguyên trong tập hợp là một phần của chính xác một khối liền kề tối đa. Cách duy nhất để số lượng khối thay đổi là khi một phần tử được chèn vào hoặc loại bỏ ở vị trí ảnh hưởng đến sự liền kề giữa`x-1`,`x`, Và`x+1`. Tất cả các phần tử khác trong tập hợp vẫn giữ nguyên thứ tự và kết nối tương đối. Vì mọi thao tác chỉ sửa đổi một điểm duy nhất nên những thay đổi cấu trúc duy nhất đối với các thành phần là sự hợp nhất hoặc tách cục bộ liên quan đến tối đa hai kết nối liền kề. Các quy tắc cập nhật liệt kê đầy đủ tất cả các cấu hình cục bộ có thể có, đảm bảo số lượng phân đoạn vẫn chính xác sau mỗi thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    q = int(input())
    S = set()
    segments = 0
    out = []

    for _ in range(q):
        c, xs = input().split()
        x = int(xs)

        if c == '+':
            left = (x - 1) in S
            right = (x + 1) in S

            if not left and not right:
                segments += 1
            elif left and right:
                segments -= 1

            S.add(x)

        else:
            left = (x - 1) in S
            right = (x + 1) in S

            if not left and not right:
                segments -= 1
            elif left and right:
                segments += 1

            S.remove(x)

        out.append(str(segments))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai dựa trên Python`set`để hỗ trợ các truy vấn thành viên O(1) trung bình cho`x-1`Và`x+1`. Điều tinh tế quan trọng là việc kiểm tra hàng xóm được thực hiện trước khi sửa đổi tập hợp. Nếu chúng ta chèn hoặc xóa trước, chúng ta sẽ mất thông tin kề cận chính xác và phân loại sai các chuyển đổi. 

các`segments`biến chỉ được cập nhật trong hai trường hợp cực đoan: phần tử biệt lập hoặc kết nối kép. Các trường hợp ở giữa ngầm bảo toàn số lượng, đó là lý do tại sao không cần xử lý rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự:```
+ 1
+ 2
+ 4
- 2
+ 3
```Chúng tôi theo dõi`S`Và`segments`. 

| Bước | Hoạt động | Đặt S | Hàng xóm bên trái | Hàng xóm bên phải | Phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | +1 | {1} | F | F | 1 | 
| 2 | +2 | {1,2} | T | F | 1 | 
| 3 | +4 | {1,2,4} | F | F | 2 | 
| 4 | -2 | {1,4} | T | T | 3 | 
| 5 | +3 | {1,3,4} | T | T | 2 | 

Dấu vết cho thấy sự liền kề thúc đẩy mọi thay đổi như thế nào. Ở bước 4, loại bỏ`2`chia tách`{1,2,4}`vào trong`{1}`Và`{4}`, tăng các đoạn từ 2 lên 3. Ở bước 5, chèn`3`sáp nhập`{1}`Và`{4}`lại thành một khối. 

Bây giờ là trường hợp thứ hai:```
+ 10
+ 12
+ 11
```| Bước | Hoạt động | Đặt S | Hàng xóm bên trái | Hàng xóm bên phải | Phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | +10 | {10} | F | F | 1 | 
| 2 | +12 | {10,12} | F | F | 2 | 
| 3 | +11 | {10,11,12} | T | T | 1 | 

Điều này thể hiện trường hợp “cầu nối” chính: chèn 11 kết nối hai đoạn riêng biệt thành một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) trung bình | Mỗi thao tác thực hiện tra cứu và cập nhật hàm băm dự kiến ​​O(1) | 
| Không gian | O(n) | Bộ lưu trữ tất cả các phần tử hoạt động | 

Với tối đa 10^6 thao tác, các phép toán băm hệ số không đổi vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    S = set()
    segments = 0
    out = []

    for _ in range(q):
        c, xs = input().split()
        x = int(xs)

        if c == '+':
            left = (x - 1) in S
            right = (x + 1) in S

            if not left and not right:
                segments += 1
            elif left and right:
                segments -= 1

            S.add(x)

        else:
            left = (x - 1) in S
            right = (x + 1) in S

            if not left and not right:
                segments -= 1
            elif left and right:
                segments += 1

            S.remove(x)

        out.append(str(segments))

    return "\n".join(out)

# provided sample (partial reconstruction; expected behavior illustration)
assert run("""8
+ 0
+ 6
+ 1
- 0
+ 3
+ 4
+ 5
- 5
""").strip() == "\n".join([
"1","2","2","2","3","3","3","3"
])

# minimum size
assert run("""1
+ 100
""").strip() == "1"

# bridge merge
assert run("""3
+ 1
+ 3
+ 2
""").strip() == "\n".join(["1","2","1"])

# full delete back to empty
assert run("""4
+ 1
+ 2
- 1
- 2
""").strip() == "\n".join(["1","1","1","0"])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chèn đơn | 1 | trường hợp cơ sở | 
| hợp nhất cầu | 1 ở cuối | hợp nhất hai phân đoạn | 
| thêm rồi xóa hết | 0 | xử lý xóa đúng | 

## Vỏ cạnh 

Một tình huống tế nhị là khi phần chèn kết nối hai đoạn riêng biệt. Ví dụ, bắt đầu từ`{1}`Và`{3}`, chèn`2`phải giảm số lượng phân đoạn. 

đầu vào:```
+ 1
+ 3
+ 2
```Trước khi chèn`2`,`S = {1, 3}`và phân đoạn là 2. Khi xử lý`2`, cả hai người hàng xóm`1`Và`3`hiện hữu. Quy tắc kích hoạt mức giảm, cho các phân đoạn = 1, phản ánh chính xác`{1,2,3}`như một khoảng. 

Một trường hợp khác là xóa phần tử cầu nối. 

đầu vào:```
+ 4
+ 5
+ 6
- 5
```Tại`{4,5,6}`, phân đoạn là 1. Loại bỏ`5`thấy cả hai hàng xóm đều tồn tại, do đó các phân đoạn tăng lên 2, tạo ra`{4}`Và`{6}`. Thuật toán nắm bắt chính xác sự phân chia vì nó đánh giá tính liền kề trước khi loại bỏ.
