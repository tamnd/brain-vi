---
title: "CF 106015N - Hạt rải rác của Sóc"
description: "Chúng ta được cho một tập hợp các số nguyên, mỗi số đại diện cho “năng lượng” của một hạt. Nhiệm vụ là chọn hai hạt riêng biệt sao cho tổng năng lượng của chúng là số lẻ và trong số tất cả các cặp hợp lệ như vậy, trả về tổng năng lượng tối đa có thể. Nếu không có cặp nào tạo ra tổng lẻ thì câu trả lời là −1."
date: "2026-06-22T16:48:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "N"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 47
verified: true
draft: false
---

[CF 106015N - Những quả hạch rải rác của con sóc](https://codeforces.com/problemset/problem/106015/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên, mỗi số đại diện cho “năng lượng” của một hạt. Nhiệm vụ là chọn hai hạt riêng biệt sao cho tổng năng lượng của chúng là số lẻ và trong số tất cả các cặp hợp lệ như vậy, trả về tổng năng lượng tối đa có thể. Nếu không có cặp nào tạo ra tổng lẻ thì câu trả lời là −1. 

Đầu ra phụ thuộc hoàn toàn vào hành vi ghép nối, không phụ thuộc vào thứ tự hoặc vị trí. Chúng tôi đang làm việc hiệu quả với nhiều tập hợp giá trị và phải suy luận xem hai phần tử nào có thể được kết hợp để đáp ứng ràng buộc chẵn lẻ trong khi tối đa hóa tổng kết quả. 

Các ràng buộc cho phép tối đa 200.000 giá trị, mỗi giá trị lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ phương pháp bậc hai nào kiểm tra rõ ràng tất cả các cặp, vì điều đó sẽ yêu cầu khoảng 4 × 10¹⁰ phép tính trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Chúng ta cần một giải pháp xử lý mảng theo thời gian tuyến tính hoặc gần tuyến tính. 

Một trường hợp khó phát hiện khi tất cả các số có cùng tính chẵn lẻ. Nếu tất cả các giá trị đều là số chẵn thì mọi tổng đều là số chẵn. Nếu tất cả đều là số lẻ thì mọi tổng cũng là số chẵn. Trong cả hai trường hợp, không có cặp hợp lệ nào tồn tại. 

Ví dụ, đầu vào`2 4 6`không tạo ra cặp tổng lẻ nên câu trả lời là −1. Một cách tiếp cận đơn giản vẫn có thể cố gắng tính tổng cặp tối đa và trả về nhầm 10. 

Một trường hợp cạnh khác xảy ra khi chỉ tồn tại một số lẻ hoặc một số chẵn. Ví dụ,`1 2 4 6`chỉ có một phần tử lẻ, do đó không thể hình thành cặp tổng lẻ hợp lệ ngay cả khi tồn tại các giá trị lớn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force thử từng cặp (i, j) và kiểm tra xem Ai + Aj có lẻ hay không. Điều này rất đơn giản: với mỗi cặp, hãy tính tổng và cập nhật câu trả lời đúng nhất nếu tổng đó là số lẻ. Tính đúng đắn là hiển nhiên vì nó liệt kê tất cả các khả năng. 

Vấn đề là quy mô. Với N lên tới 200.000, số cặp là khoảng N(N−1)/2, tức là khoảng 2 × 10¹⁰ phép tính trong trường hợp xấu nhất. Điều này không thể chạy trong thời gian giới hạn. 

Quan sát quan trọng đến từ cấu trúc chẵn lẻ. Một tổng là số lẻ khi và chỉ khi một số chẵn và số kia là số lẻ. Điều này làm giảm vấn đề từ “tất cả các cặp” thành “chỉ các cặp chẵn lẻ chéo”. Trong số tất cả các cặp hợp lệ, tổng tối đa đạt được bằng cách lấy giá trị chẵn lớn nhất và giá trị lẻ lớn nhất. 

Điều này biến vấn đề thành một lần quét đơn giản: theo dõi phần tử chẵn tối đa và phần tử lẻ tối đa, sau đó tính tổng của chúng nếu cả hai đều tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách trích xuất cấu trúc được áp đặt bởi tính chẵn lẻ và giảm không gian tìm kiếm xuống còn hai ứng cử viên. 

1. Quét qua mảng một lần trong khi vẫn duy trì hai biến: giá trị chẵn tối đa được thấy cho đến nay và giá trị lẻ tối đa được thấy cho đến nay. Chúng tôi khởi tạo cả hai dưới dạng vắng mặt, ví dụ như sử dụng trọng điểm như −1. 
2. Với mỗi số Ai, hãy xác định tính chẵn lẻ của nó. Nếu Ai là số chẵn, hãy so sánh nó với giá trị chẵn tốt nhất hiện tại và cập nhật nó nếu Ai lớn hơn. Nếu Ai là số lẻ, hãy làm tương tự với bộ theo dõi số lẻ. Điều này đảm bảo chúng tôi luôn giữ được ứng viên tốt nhất có thể trong mỗi lớp chẵn lẻ. 
3. Sau khi xử lý tất cả các giá trị, hãy kiểm tra xem có tồn tại cả giá trị chẵn tối đa và giá trị lẻ tối đa hay không. Nếu thiếu một trong hai thì không có cặp hợp lệ nào có thể tạo thành tổng lẻ, vì vậy câu trả lời là −1. 
4. Nếu cả hai đều tồn tại, hãy tính tổng của chúng. Đây là cấu trúc duy nhất đảm bảo tính lẻ, vì chẵn + lẻ là cách duy nhất để tạo ra kết quả lẻ. 

Lựa chọn chỉ giữ cực đại là hợp lý vì bất kỳ giải pháp hợp lệ nào cũng phải ghép một số lẻ và một số chẵn và việc tối đa hóa tổng một cách độc lập trong mỗi nhóm chẵn lẻ sẽ mang lại cặp tốt nhất có thể. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán bảo toàn số chẵn lớn nhất được nhìn thấy và số lẻ lớn nhất được nhìn thấy. Bất kỳ cặp tổng lẻ hợp lệ nào cũng phải bao gồm một phần tử từ mỗi lớp chẵn lẻ. Việc thay thế một trong hai phần tử trong một cặp như vậy bằng giá trị lớn hơn có cùng tính chẵn lẻ chỉ có thể làm tăng tổng. Do đó, cặp tối ưu được hình thành bởi các giá trị chẵn và lẻ tối đa trong mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    max_even = -1
    max_odd = -1
    
    for x in arr:
        if x % 2 == 0:
            if x > max_even:
                max_even = x
        else:
            if x > max_odd:
                max_odd = x
    
    if max_even == -1 or max_odd == -1:
        print(-1)
    else:
        print(max_even + max_odd)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai cực đại đang chạy trong khi quét đầu vào một lần. Giá trị trọng điểm −1 hoạt động an toàn vì tất cả đầu vào ít nhất là 1, do đó không thể nhầm lẫn nó với giá trị mảng hợp lệ. 

Một lỗi phổ biến là cố gắng chỉ theo dõi tổng mà không tách các lớp chẵn lẻ. Điều đó không thành công vì nó bỏ qua ràng buộc về cấu trúc mà tổng lẻ yêu cầu tính chẵn lẻ ngược lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 3 4`Chúng tôi theo dõi từng bước các giá trị chẵn và lẻ tối đa. 

| Yếu tố | Chẵn lẻ | tối đa_thậm chí | tối đa_lẻ | 
| --- | --- | --- | --- | 
| 1 | lẻ | -1 | 1 | 
| 2 | thậm chí | 2 | 1 | 
| 3 | lẻ | 2 | 3 | 
| 4 | thậm chí | 4 | 3 | 

Số chẵn tối đa cuối cùng là 4 và số lẻ tối đa là 3, vì vậy câu trả lời là 7. 

Điều này chứng tỏ rằng các giá trị trung gian không quan trọng ngoài vai trò của chúng trong việc cập nhật các ứng viên tốt nhất. 

### Ví dụ 2 

đầu vào:`2 4 6 8`| Yếu tố | Chẵn lẻ | tối đa_thậm chí | tối đa_lẻ | 
| --- | --- | --- | --- | 
| 2 | thậm chí | 2 | -1 | 
| 4 | thậm chí | 4 | -1 | 
| 6 | thậm chí | 6 | -1 | 
| 8 | thậm chí | 8 | -1 | 

Không có giá trị lẻ nào tồn tại, do đó không thể hình thành cặp hợp lệ và đầu ra là −1. 

Điều này xác nhận tính đúng đắn của việc kiểm tra tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | truyền một lần qua mảng, công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | chỉ có hai biến số nguyên được duy trì | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả kích thước đầu vào tối đa cũng chỉ yêu cầu 200.000 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-style cases
assert run("4\n1 2 3 4\n") == "7"
assert run("4\n2 4 6 8\n") == "-1"

# minimum size
assert run("2\n1 2\n") == "3"

# all odds
assert run("5\n1 3 5 7 9\n") == "-1"

# large mixed
assert run("6\n10 20 1 3 7 8\n") == "28"

# duplicate max parity values
assert run("5\n2 2 2 1 100\n") == "101"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4, 1 2 3 4 | 7 | lựa chọn chẵn lẻ hỗn hợp bình thường | 
| 4, 2 4 6 8 | -1 | không tồn tại cặp tổng lẻ | 
| 2, 1 2 | 3 | đầu vào hợp lệ tối thiểu | 
| tất cả tỷ lệ cược | -1 | trường hợp không hợp lệ chỉ có tính chẵn lẻ | 
| hỗn hợp lớn | 28 | lựa chọn tối đa chính xác trên toàn bộ phân phối | 
| trùng lặp | 101 | xử lý cực đại lặp lại một cách chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng chỉ chứa các số chẵn. Đối với đầu vào`6\n2 4 6 8 10 12`, quá trình quét đặt max_even thành 12 trong khi max_odd vẫn giữ nguyên −1 trong suốt quá trình quét. Cuối cùng, thuật toán xuất ra chính xác −1 vì không thể ghép cặp chẵn lẻ chéo. 

Một trường hợp cạnh khác là khi có chính xác một phần tử của lớp chẵn lẻ. Đối với đầu vào`4\n100 2 3 4`, max_even trở thành 100 và max_odd trở thành 3. Thuật toán vẫn hoạt động vì chỉ cần có một số lẻ hoặc số chẵn miễn là lớp kia tồn tại. 

Trường hợp thứ ba là khi số chẵn lớn nhất và số lẻ lớn nhất xuất hiện muộn trong mảng. Đối với đầu vào`5\n1 8 3 6 7`, cực đại đang chạy sẽ phát triển cho đến khi max_even = 8 và max_odd = 7 ở cuối. Câu trả lời cuối cùng 15 khẳng định rằng thứ tự không ảnh hưởng đến tính đúng đắn, chỉ ảnh hưởng đến giá trị cực đại.
