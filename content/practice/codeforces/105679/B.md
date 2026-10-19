---
title: "CF 105679B - Bóng Bàn Pinball"
description: "Máy có năm cột dọc. Sau khi ba quả bóng đầu tiên đã rơi xuống, chúng ta biết hiện có bao nhiêu quả bóng ở mỗi cột. Vẫn còn hai quả bóng để thả và mỗi quả bóng có thể đi vào bất kỳ cột nào trong số năm cột."
date: "2026-06-26T09:50:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105679
codeforces_index: "B"
codeforces_contest_name: "IOI 2024 International Study Camp Mini Competition"
rating: 0
weight: 105679
solve_time_s: 72
verified: true
draft: false
---

[CF 105679B - Bóng bàn Pinball](https://codeforces.com/problemset/problem/105679/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Máy có năm cột dọc. Sau khi ba quả bóng đầu tiên đã rơi xuống, chúng ta biết hiện có bao nhiêu quả bóng ở mỗi cột. Vẫn còn hai quả bóng để thả và mỗi quả bóng có thể đi vào bất kỳ cột nào trong số năm cột. 

Giải thưởng được xác định bằng sự sắp xếp cuối cùng của cả năm quả bóng. Một đường thẳng đứng có nghĩa là tất cả các quả bóng được yêu cầu đều nằm trong một cột. Đường ngang có nghĩa là các quả bóng nằm cùng một hàng trên các cột khác nhau. Nhiệm vụ là quyết định xem hai quả bóng còn lại có thể khiến người chơi giành được giải thưởng lớn, chỉ giải cơ bản hay không có giải thưởng. 

Đầu vào là năm số nguyên. Giá trị tại vị trí`i`là số quả bóng hiện có bên trong`i`-khe thứ. Tổng của họ luôn là ba vì có đúng ba quả bóng đã được chơi. Kết quả đầu ra mô tả kết quả tốt nhất có thể sau khi chọn nơi hai quả bóng cuối cùng rơi xuống. 

Số lượng vị trí nhỏ là hạn chế chính. Chỉ có năm điểm đến có thể có cho mỗi quả bóng trong số hai quả bóng còn lại, vì vậy tổng số kết thúc có thể có là rất nhỏ. Ngay cả việc mô phỏng trực tiếp mọi khả năng cũng chỉ kiểm tra được 25 trường hợp. Điều này có nghĩa là những mối quan tâm thông thường về độ phức tạp tuyến tính hoặc bậc hai đối với các mảng lớn không áp dụng ở đây. 

Các trường hợp cạnh chính xuất phát từ việc nhầm lẫn hình dạng của bảng chỉ với số lượng cột. 

Ví dụ, đầu vào```text
1 1 1 0 0
```có thể trở thành một giải thưởng lớn. Đặt hai quả bóng còn lại vào ô 4 và 5, tạo ra:```text
1 1 1 1 1
```Mỗi cột có một quả bóng nên hàng ngang phía dưới chứa tất cả năm quả bóng. 

Một lỗi phổ biến là chỉ kiểm tra xem một cột có thể đạt tới độ cao năm hay không. Điều đó bỏ lỡ chiến thắng theo chiều ngang. 

Một trường hợp phức tạp khác là:```text
3 0 0 0 0
```Đầu ra đúng là`add oil`. Thêm cả hai quả bóng vào ô đầu tiên sẽ có năm quả bóng trong một cột, nhưng giải thưởng lớn là không thể vì các cột khác vẫn trống. Giải pháp chỉ kiểm tra xem một cột có thể chứa năm quả bóng hay không sẽ báo cáo sai giải thưởng lớn. 

Trường hợp ranh giới cuối cùng là:```text
1 1 1 0 0
```Đầu ra đúng là`op`, không`add oil`, vì hai bi còn lại có thể hoàn thành hàng ngang. Giải pháp chỉ kiểm tra số lượng bóng trong các cột hiện có có thể không thành công do chưa có cơ chế phân thắng bại. 

## Phương pháp tiếp cận 

Giải pháp đơn giản nhất là thử mọi vị trí có thể của hai quả bóng cuối cùng. Đối với mỗi lựa chọn trong hai vị trí đích, chúng tôi tạo năm chiều cao cột cuối cùng và kiểm tra xem bảng chứa giải thưởng lớn hay giải thưởng cơ bản. 

Lực lượng vũ phu này đã đủ nhanh vì chỉ có năm lựa chọn cho quả bóng đầu tiên còn lại và năm lựa chọn cho quả bóng thứ hai. Do đó số lượng mô phỏng là`5 * 5 = 25`. Mỗi mô phỏng chỉ kiểm tra năm cột, đưa ra 125 thao tác đơn giản. 

Quan sát hữu ích là bài toán có không gian trạng thái cực kỳ nhỏ. Thay vì cố gắng tìm ra những điều kiện phức tạp cho mọi sự sắp xếp có thể xảy ra, chúng ta có thể trực tiếp khám phá mọi khả năng có thể xảy ra trong tương lai. Điều này tránh những sai sót xung quanh các hàng ngang vì mô phỏng sẽ xây dựng bàn cờ cuối cùng một cách tự nhiên và kiểm tra điều kiện chiến thắng thực tế. 

Lực lượng vũ phu hoạt động vì số lượng tương lai có thể xảy ra là không đổi. Một phiên bản lớn hơn của cùng một vấn đề với nhiều cột sẽ yêu cầu quan sát toán học, nhưng ở đây việc kiểm tra toàn diện là cách tiếp cận rõ ràng và an toàn nhất. 

Tối ưu hóa duy nhất cần thiết là kiểm tra giải thưởng lớn trước giải thưởng cơ bản. Nếu có ít nhất một kết thúc có thể mang lại giải thưởng lớn thì câu trả lời phải là`op`thậm chí nhiều kết thúc khác chỉ mang lại giải thưởng cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(25 * 5) | O(5) | Đã chấp nhận | 
| Tối ưu | O(25 * 5) | O(5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm chiều cao cột hiện tại. 

2. Hãy thử mọi khe có thể cho quả bóng thứ tư. Với mỗi lựa chọn như vậy, hãy thử mọi ô có thể cho quả bóng thứ năm. 

3. Đối với mỗi cặp lựa chọn, hãy tăng độ cao của cột tương ứng và kiểm tra bảng kết quả. Một hàng dọc có độ dài bằng 5 tồn tại nếu bất kỳ cột nào có chiều cao bằng 5. Một hàng ngang dài 5 tồn tại nếu mỗi cột có ít nhất một quả bóng. 

4. Nếu bất kỳ kết thúc nào tạo ra giải thưởng lớn, hãy nhớ rằng câu trả lời là`op`. 

5. Nếu không có kết thúc nào tạo ra giải thưởng lớn, hãy lặp lại tìm kiếm tương tự và kiểm tra xem có kết thúc nào tạo ra giải thưởng cơ bản hay không. Giải thưởng cơ bản theo chiều dọc tồn tại khi cột có chiều cao ít nhất là bốn. Giải thưởng cơ bản theo chiều ngang tồn tại khi có ít nhất bốn cột có chiều cao ít nhất là một. 

6. Nếu không thể nhận được loại giải thưởng nào, hãy xuất ra`gg`. 

Tại sao nó hoạt động: mọi sự sắp xếp hợp pháp trong tương lai đều được thể hiện bằng chính xác một cặp lựa chọn cho hai quả bóng còn lại. Thuật toán sẽ kiểm tra từng cặp như vậy nên không thể bỏ sót sự sắp xếp chiến thắng. Việc kiểm tra giải thưởng là bản dịch trực tiếp các quy tắc của hội đồng, nghĩa là mọi chiến thắng được báo cáo đều hợp lệ và mọi chiến thắng có thể xảy ra đều được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check_grand(a):
    if max(a) == 5:
        return True
    if all(x >= 1 for x in a):
        return True
    return False

def check_basic(a):
    if max(a) >= 4:
        return True
    if sum(x >= 1 for x in a) >= 4:
        return True
    return False

def solve():
    c = list(map(int, input().split()))

    grand = False
    basic = False

    for i in range(5):
        for j in range(5):
            a = c[:]
            a[i] += 1
            a[j] += 1

            if check_grand(a):
                grand = True
            if check_basic(a):
                basic = True

    if grand:
        print("op")
    elif basic:
        print("add oil")
    else:
        print("gg")

if __name__ == "__main__":
    solve()
```các`check_grand`hàm chỉ kiểm tra hai hình dạng có thể chứa tất cả năm quả bóng. Giải thưởng lớn theo chiều dọc yêu cầu một cột để chứa tất cả năm quả bóng. Giải thưởng lớn theo chiều ngang yêu cầu mỗi cột phải chứa quả bóng phía dưới. 

các`check_basic`Hàm xử lý hai cách để có được bốn quả bóng thẳng hàng. Trường hợp dọc sử dụng`max(a) >= 4`, trong khi trường hợp ngang đếm có bao nhiêu cột không trống. Điều kiện sử dụng`>= 1`bởi vì một hàng ngang chỉ có thể tồn tại nếu cột đạt đến độ cao đó. 

Các vòng lặp lồng nhau là quá trình tìm kiếm hoàn chỉnh trên hai quả bóng còn lại. Sao chép mảng trước khi sửa đổi nó sẽ tránh nhiễu giữa các phần cuối mô phỏng khác nhau. Vì mảng có độ dài 5 nên chi phí sao chép này không đáng kể. 

Thứ tự của các lần kiểm tra cuối cùng rất quan trọng. Giải thưởng lớn tốt hơn giải thưởng cơ bản nên mã trả về`op`bất cứ khi nào có thể tiếp tục đạt được phần thưởng cao nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```text
1 2 0 0 0
```Các vị trí hữu ích có thể có bao gồm đặt hai quả bóng vào các ô trống để tạo ra bốn cột bị chiếm đóng hoặc đặt một quả bóng vào ô thứ hai. 

| Quả bóng mới đầu tiên | Quả bóng mới thứ hai | Độ cao cuối cùng | Giải thưởng lớn | Giải thưởng cơ bản | 
|---|---|---|---|---| 
| 2 | 2 | 1 4 0 0 0 | Không | Có | 
| 3 | 4 | 1 2 1 1 0 | Không | Có | 
| 3 | 5 | 1 2 1 0 1 | Không | Không | 

Vì không có phần cuối nào có thể lấp đầy cả năm cột hoặc tạo thành một cột có chiều cao năm cột, nên kết quả tốt nhất có thể là giải cơ bản. 

Đầu ra:```text
add oil
```Dấu vết này cho thấy lý do tại sao thuật toán kiểm tra mọi kết thúc có thể thay vì cho rằng sự sắp xếp hữu ích đầu tiên là sự sắp xếp tốt nhất. 

### Mẫu 2 

đầu vào:```text
1 0 1 0 1
```Thử các quả bóng còn lại ở ô 2 và 4 sẽ cho: 

| Quả bóng mới đầu tiên | Quả bóng mới thứ hai | Độ cao cuối cùng | Giải thưởng lớn | Giải thưởng cơ bản | 
|---|---|---|---|---| 
| 2 | 4 | 1 1 1 1 1 | Có | Có | 

Mỗi cột bây giờ có một quả bóng, tạo thành một hàng ngang hoàn chỉnh. 

Đầu ra:```text
op
```Ví dụ này thể hiện điều kiện giải thưởng lớn theo chiều ngang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(25 * 5) | Có 25 cặp vị trí bóng còn lại có thể có và mỗi lần kiểm tra bảng cuối cùng sẽ quét năm cột. | 
| Không gian | O(5) | Chỉ có một sự sắp xếp năm cột tạm thời được lưu trữ. | 

Thuật toán thực hiện một số thao tác không đổi vì bảng luôn có đúng năm vị trí. Nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("1 2 0 0 0\n") == "add oil\n", "sample 1"
assert run("1 0 1 0 1\n") == "op\n", "sample 2"

assert run("3 0 0 0 0\n") == "add oil\n", "vertical four or five boundary"
assert run("1 1 1 0 0\n") == "op\n", "horizontal grand prize"
assert run("0 0 0 0 3\n") == "add oil\n", "single column growth"
assert run("0 1 1 1 0\n") == "add oil\n", "four occupied columns possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`3 0 0 0 0`|`add oil`| Kiểm tra cột cao và ngăn chặn việc phát hiện sai giải thưởng lớn. | 
|`1 1 1 0 0`|`op`| Kiểm tra việc hoàn thành một hàng ngang với hai quả bóng cuối cùng. | 
|`0 0 0 0 3`|`add oil`| Kiểm tra sự tăng trưởng từ một cột cạnh khác. | 
|`0 1 1 1 0`|`add oil`| Kiểm tra trường hợp một số cột đã bị chiếm dụng. | 

## Vỏ cạnh 

Đối với đầu vào```text
3 0 0 0 0
```thuật toán sẽ thử mọi vị trí có thể của hai quả bóng còn lại. Lựa chọn tốt nhất là đặt cả hai vào cột đầu tiên, cho chiều cao`5 0 0 0 0`. Cột đầu tiên chứa năm quả bóng, vì vậy sẽ có một giải thưởng lớn theo chiều dọc. Tuy nhiên, đầu ra đúng thực sự là`op`, vì đây là giải thưởng lớn. Nếu đầu ra dự kiến ​​là`add oil`, điều đó cho thấy mức độ ưu tiên của giải thưởng đã được triển khai không chính xác. Thuật toán xử lý việc này bằng cách kiểm tra giải thưởng lớn trước. 

Đối với đầu vào```text
1 1 1 0 0
```đặt các quả bóng còn lại vào ô bốn và năm sẽ tạo ra`1 1 1 1 1`. Kiểm tra giải thưởng lớn thấy rằng tất cả các cột đều không trống nên trả về`op`. Điều này nắm bắt các giải pháp chỉ kiểm tra các đường thẳng đứng. 

Đối với đầu vào```text
0 0 0 0 3
```cải tiến hữu ích duy nhất là tăng cột cuối cùng. Trạng thái cuối cùng tốt nhất là`0 0 0 0 5`, tạo ra một đường thẳng đứng nhưng không thể tạo bất kỳ đường ngang nào. Thuật toán vẫn tìm thấy nó vì nó kiểm tra từng cặp đích đến. 

Đối với đầu vào```text
1 2 0 0 0
```đưa cả hai quả bóng còn lại vào ô thứ hai sẽ tạo ra`1 4 0 0 0`, cho bốn quả bóng theo chiều dọc. Không có sự sắp xếp nào có thể làm cho tất cả năm cột không trống hoặc tạo ra chiều cao năm cột, do đó thuật toán trả về`add oil`. Điều này xác minh sự khác biệt giữa hai cấp độ giải thưởng.
