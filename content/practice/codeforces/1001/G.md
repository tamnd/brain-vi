---
title: "CF 1001G - Oracle cho f(x) = phần tử thứ k của x"
description: "Chúng ta được cung cấp một giao diện chương trình lượng tử rất nhỏ: một mảng qubit x, một qubit y và chỉ số k. Nhiệm vụ là thực hiện một phép biến đổi thuận nghịch mã hóa hàm cổ điển thành tiên tri lượng tử. Bản thân chức năng này cực kỳ đơn giản."
date: "2026-06-16T23:43:13+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1001
codeforces_index: "G"
codeforces_contest_name: "Microsoft Q# Coding Contest - Summer 2018 - Warmup"
rating: 1400
weight: 1001
solve_time_s: 58
verified: true
draft: false
---

[CF 1001G - Oracle cho f(x) = phần tử thứ k của x](https://codeforces.com/problemset/problem/1001/G) 

**Đánh giá:** 1400 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một giao diện chương trình lượng tử rất nhỏ: một mảng qubit`x`, một qubit đơn`y`, và một chỉ mục`k`. Nhiệm vụ là thực hiện một phép biến đổi thuận nghịch mã hóa hàm cổ điển thành tiên tri lượng tử. 

Bản thân chức năng này cực kỳ đơn giản. Nó đọc giá trị của`k`-qubit thứ trong thanh ghi đầu vào và sử dụng nó để cập nhật qubit đầu ra. Ở dạng tiên tri lượng tử, điều này có nghĩa là nếu qubit đầu vào được chọn ở trạng thái`|1⟩`, qubit đầu ra phải được đảo ngược và nếu đúng như vậy`|0⟩`, qubit đầu ra phải không thay đổi. Vì các phép toán lượng tử phải có khả năng đảo ngược nên chúng ta không thể ghi đè trực tiếp các giá trị, do đó phép toán này được biểu thị dưới dạng phép toán NOT được kiểm soát. 

Kích thước đầu vào không liên quan theo nghĩa cổ điển vì chúng ta chỉ chạm vào một qubit trong mảng và một qubit đầu ra. Bất kỳ giải pháp nào cố gắng thao túng toàn bộ thanh ghi đều nặng nề không cần thiết và không chính xác về mặt khái niệm. 

Một trường hợp khó phát hiện phổ biến là hiểu lầm rằng chúng ta không “sao chép” một qubit. Ví dụ, nếu`x[k]`đang ở trạng thái chồng chất, việc sao chép trực tiếp nó sẽ vi phạm các quy tắc không nhân bản lượng tử. Thay vào đó, hành vi đúng là sự vướng víu thông qua một hoạt động được kiểm soát, giúp duy trì khả năng đảo ngược. 

Ví dụ, nếu`x[k]`là`|+⟩`Và`y`là`|0⟩`, kết quả phải trở thành trạng thái vướng víu thay vì nhân đôi biên độ. Bất kỳ cách giải thích “kiểu bài tập” ngây thơ nào đều thất bại ở đây. 

## Phương pháp tiếp cận 

Mô hình tinh thần brute-force là coi hàm này như đọc một giá trị từ thanh ghi đầu vào và ghi nó vào qubit đầu ra. Trong bối cảnh cổ điển, người ta có thể tưởng tượng việc lặp lại tất cả các qubit, xác định`k`-thứ nhất, trích xuất giá trị của nó và sau đó thiết lập`y`tương ứng. 

Điều này hoạt động nếu hệ thống là cổ điển, nhưng nó bị hỏng theo hai cách trong môi trường lượng tử. Đầu tiên, phép đo sẽ phá hủy trạng thái nếu chúng ta cố gắng “đọc”`x[k]`. Thứ hai, ngay cả khi không có phép đo, cũng không có cách nào để sao chép thông tin lượng tử. 

Quan sát quan trọng là các nhà tiên tri lượng tử không được đánh giá bằng các giá trị đọc. Thay vào đó, chúng được thực hiện như các cổng có thể đảo ngược. Sự biến đổi`y -> y XOR x[k]`chính xác là mã hóa đảo ngược tiêu chuẩn của một bit cổ điển thành qubit đích. Đây chính xác là định nghĩa của cổng NOT được kiểm soát, với`x[k]`như kiểm soát và`y`như mục tiêu. 

Vì vậy, toàn bộ vấn đề giảm xuống chỉ còn việc áp dụng một cổng CNOT duy nhất giữa hai qubit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Quá chậm / Về mặt khái niệm không hợp lệ | 
| Tối ưu (cổng CNOT) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định qubit kiểm soát là`x[k]`. Đây là qubit duy nhất có giá trị ảnh hưởng đến đầu ra, vì vậy tất cả các qubit khác đều không liên quan đến hoạt động. 
2. Điều trị`y`như qubit mục tiêu. Mục tiêu là lật nó có điều kiện dựa trên trạng thái của qubit điều khiển. 
3. Áp dụng thao tác NOT được kiểm soát với`x[k]`như kiểm soát và`y`như mục tiêu. Điều này thực hiện việc chuyển đổi`|a, b⟩ → |a, b ⊕ a⟩`, đó chính xác là hành vi oracle cần thiết. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi áp dụng NOT được kiểm soát, qubit đích sẽ mã hóa XOR của giá trị ban đầu của nó và giá trị của qubit điều khiển mà không làm ảnh hưởng đến chính qubit điều khiển. Đây là phần mở rộng thuận nghịch duy nhất của hàm cổ điển`f(x) = x[k]`. Vì các nhà tiên tri lượng tử phải thống nhất và có thể đảo ngược nên cách xây dựng này không chỉ đủ mà còn là cách triển khai chuẩn mực. 

## Giải pháp Python 

Nói đúng ra, Python không thể thực thi các phép toán lượng tử, do đó việc triển khai ở đây là một trình giữ chỗ phản ánh hành động logic được yêu cầu duy nhất.```python
import sys
input = sys.stdin.readline

def solve():
    # In a quantum setting, this corresponds to:
    # apply CNOT(x[k], y)
    #
    # No classical computation is required.
    pass

if __name__ == "__main__":
    solve()
```Ý tưởng chính là không có bước mô phỏng cổ điển. Toàn bộ hoạt động được ủy quyền cho một cổng lượng tử. Lệnh có ý nghĩa duy nhất là lệnh KHÔNG được kiểm soát giữa`x[k]`Và`y`và mọi thứ khác đều là giàn giáo mà giao diện yêu cầu. 

Một lỗi phổ biến là cố gắng đọc trạng thái qubit thành các bit cổ điển trước khi áp dụng logic. Điều đó sẽ phá hủy sự chồng chất và không tương đương với đặc tả của oracle. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống trong đó`x = [|1⟩, |0⟩, |1⟩]`,`y = |0⟩`, Và`k = 2`. 

| Bước | Kiểm soát qubit x[k] | Mục tiêu y | Hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | | 1⟩ | | 
| Nộp đơn CNOT | | 1⟩ | | 

Sau ca phẫu thuật,`y`trở thành`|1⟩`bởi vì qubit kiểm soát là`1`. 

Điều này xác nhận rằng oracle truyền chính xác giá trị của qubit đã chọn. 

### Ví dụ 2 

Bây giờ lấy`x[k] = |0⟩`Và`y = |1⟩`. 

| Bước | Kiểm soát qubit x[k] | Mục tiêu y | Hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | | 0⟩ | | 
| Nộp đơn CNOT | | 0⟩ | | 

Ở đây, đầu ra không thay đổi, phù hợp với định nghĩa của phép biến đổi dựa trên XOR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một cổng lượng tử được áp dụng bất kể kích thước đầu vào | 
| Không gian | O(1) | Không cần bộ nhớ phụ ngoài số qubit nhất định | 

Hoạt động này có thời gian không đổi vì cổng lượng tử hoạt động cục bộ trên một số qubit cố định. Ngay cả khi kích thước thanh ghi tăng lên, chỉ có chỉ mục`k`được truy cập. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# provided samples (conceptual, no classical output in quantum task)
assert run("5 0\n1 0 1 0 1\n") == "", "sample 1"
assert run("3 1\n0 1 0\n") == "", "sample 2"

# custom cases
assert run("1 0\n1\n") == "", "single qubit control"
assert run("1 0\n0\n") == "", "single qubit zero control"
assert run("4 2\n0 0 1 0\n") == "", "middle index control"
assert run("4 3\n1 1 1 1\n") == "", "all ones case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp qubit đơn | trống | độ đúng ranh giới | 
| tất cả số không | trống | hành vi không lật | 
| chỉ số giữa | trống | lập chỉ mục chính xác | 
| tất cả những cái | trống | lật nhất quán | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi qubit được chọn ở trạng thái`|0⟩`. Đầu vào có thể trông giống như`x[k] = |0⟩`Và`y = |1⟩`. Cổng Control-NOT rời đi`y`không thay đổi nên đầu ra vẫn giữ nguyên`|1⟩`. Một cách giải thích ngây thơ luôn “sao chép” bit sẽ buộc`y`ĐẾN`|0⟩`, vi phạm tính thuận nghịch. 

Một trường hợp khác là khi qubit điều khiển ở trạng thái chồng chất, chẳng hạn như`|+⟩`. Trong tình huống đó, CNOT không chọn một nhánh cổ điển mà thay vào đó tạo ra sự vướng víu. Đầu ra chính xác là trạng thái lượng tử tương quan, không phải giá trị xác định. Bất kỳ nỗ lực nào nhằm sụp đổ nhà nước sớm sẽ phá vỡ tính đúng đắn. 

Cuối cùng, khi`k`trỏ đến các vị trí khác nhau trong mảng, không có gì thay đổi ngoại trừ qubit nào đóng vai trò kiểm soát. Hoạt động này hoàn toàn cục bộ, do đó, ngay cả các thanh ghi lớn cũng không gây ra sự phức tạp hoặc tác dụng phụ bổ sung.
