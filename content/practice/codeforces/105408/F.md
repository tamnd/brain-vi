---
title: "CF 105408F - Thiếu đồ chơi công bằng"
description: "Chúng tôi được cung cấp hai bộ sưu tập nhỏ các mã nhận dạng đồ chơi. Túi của Alice chứa năm món đồ chơi, và túi của Bob được cho là chứa chính xác bộ đồ chơi đó nhưng thiếu một món đồ nên túi của Bob chỉ có bốn món."
date: "2026-06-24T23:08:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105408
codeforces_index: "F"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico Repechaje"
rating: 0
weight: 105408
solve_time_s: 63
verified: false
draft: false
---

[CF 105408F - Thiếu đồ chơi công bằng](https://codeforces.com/problemset/problem/105408/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai bộ sưu tập nhỏ các mã nhận dạng đồ chơi. Túi của Alice chứa năm món đồ chơi, và túi của Bob được cho là chứa chính xác bộ đồ chơi đó nhưng thiếu một món đồ nên túi của Bob chỉ có bốn món. Mọi đồ chơi đều được biểu thị bằng một số nguyên và tất cả các giá trị trong danh sách của Alice đều được đảm bảo xuất hiện trong danh sách của Bob ngoại trừ chính xác một giá trị. 

Nhiệm vụ là xác định số nguyên nào xuất hiện trong danh sách của Alice nhưng không xuất hiện trong danh sách của Bob. 

Kích thước đầu vào rất nhỏ, chỉ có năm và bốn số. Điều đó ngay lập tức loại bỏ mọi lo ngại về hạn chế hiệu suất. Bất kỳ giải pháp nào thậm chí quét dữ liệu nhiều lần sẽ phù hợp thoải mái trong giới hạn thời gian, vì vậy vấn đề hoàn toàn nằm ở việc trích xuất mối quan hệ logic chính xác giữa hai tập hợp. 

Một số tình huống khó khăn đáng lưu ý. Một là khi đồ chơi bị thiếu có giá trị nhỏ nhất hoặc lớn nhất trong bộ, ví dụ Alice có`1 2 3 4 5`và Bob có`2 3 4 5`, đáp án ở đâu`1`. Một trường hợp khác là khi các giá trị không được sắp xếp hoặc bị xen kẽ, ví dụ Alice có`10 14 22 32 110`và Bob có`14 22 32 110`, trong đó giá trị còn thiếu là`10`. Một cách tiếp cận bất cẩn giả định việc sắp xếp thứ tự hoặc cố gắng khớp các vị trí sẽ thất bại ngay lập tức vì không có sự đảm bảo về đầu vào được sắp xếp hoặc căn chỉnh giữa các vị trí. 

## Phương pháp tiếp cận 

Một cách trực tiếp nhưng không hiệu quả để giải quyết vấn đề là lấy từng phần tử từ danh sách của Alice và kiểm tra xem nó có tồn tại trong danh sách của Bob hay không bằng cách quét tuyến tính. Đối với mỗi phần tử trong số năm phần tử, chúng tôi có thể quét tối đa bốn phần tử trong danh sách của Bob. Điều này dẫn đến số lượng thao tác không đổi, khoảng 20 phép so sánh, vốn đã nhỏ. Mặc dù ý tưởng mạnh mẽ này đúng về mặt logic, nhưng việc cấu trúc nó thành tìm kiếm lặp đi lặp lại là không cần thiết. 

Quan điểm rõ ràng hơn xuất phát từ việc nhận thấy rằng chúng ta đang so sánh hai tập hợp gần như giống hệt nhau trong đó chỉ có một giá trị khác nhau về tần số. Thay vì tìm kiếm phần tử bị thiếu một cách rõ ràng, chúng ta có thể so sánh cấu trúc tổng hợp. Một bất biến đơn giản là tổng của tất cả các phần tử trong túi của Alice lớn hơn tổng của túi của Bob đúng bằng giá trị của món đồ chơi bị thiếu. Điều này có tác dụng vì tất cả các phần tử chung đều bị loại bỏ trong phép trừ. 

Một quan điểm tương đương khác là sử dụng XOR. Vì các giá trị giống hệt nhau bị hủy theo XOR, nên việc XOR tất cả các phần tử của túi Alice với tất cả các phần tử của túi Bob chỉ để lại giá trị còn thiếu. Cả hai cách tiếp cận đều dựa trên cùng một nguyên tắc hủy, nhưng phép tính tổng mang tính trực quan tức thì hơn trong cài đặt ràng buộc này. 

Việc giảm thiểu chính là chuyển từ kiểm tra thành viên sang so sánh tổng hợp. Khi chúng tôi chấp nhận rằng tất cả các phần tử dùng chung sẽ biến mất khi thực hiện phép trừ hoặc XOR, bài toán sẽ chuyển thành một phép toán số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | O(5 × 4) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tổng chênh lệch | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng phương pháp tổng chênh lệch vì đây là cách giải thích trực tiếp nhất về ý tưởng hủy bỏ. 

1. Đọc năm số nguyên từ túi của Alice và tính tổng của chúng. Tổng này đại diện cho tổng trọng lượng của tất cả các đồ chơi dự kiến. 
2. Đọc bốn số nguyên từ túi của Bob và tính tổng của chúng. Tổng này đại diện cho tổng số giảm sau khi thiếu một mục. 
3. Trừ tổng của Bob khỏi tổng của Alice. Kết quả của phép trừ này tách biệt chính xác giá trị đồ chơi bị thiếu. 
4. Xuất ra chênh lệch được tính toán. 

Lý do phép trừ có hiệu quả ở đây là vì mọi đồ chơi dùng chung xuất hiện chính xác một lần trong cả hai tổng và do đó loại bỏ hoàn toàn. 

### Tại sao nó hoạt động 

Đặt tập hợp đầy đủ là A và tập hợp của Bob là B. B giống hệt A ngoại trừ một giá trị x bị loại bỏ. Khi chúng ta tính sum(A) − sum(B), mọi phần tử dùng chung đóng góp như nhau cho cả hai tổng và biến mất trong phép trừ. Chỉ có x là chưa bị hủy nên kết quả phải bằng x. Vì bài toán đảm bảo chính xác một phần tử bị thiếu và không có sửa đổi bổ sung nào nên bất biến này có giá trị xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    return str(sum(a) - sum(b))

if __name__ == "__main__":
    print(solve())
```Giải pháp đọc cả hai dòng, chuyển đổi chúng thành danh sách số nguyên và tính tổng của chúng. Bước trừ là cốt lõi của logic, trực tiếp thực hiện đối số hủy được mô tả trước đó. Đầu ra được trả về dưới dạng một chuỗi để in rõ ràng. 

Một chi tiết triển khai tinh tế là đảm bảo rằng đầu vào được phân chia chính xác ngay cả khi khoảng cách không đều. sử dụng`split()`xử lý tất cả khoảng trắng một cách thống nhất, do đó không có nguy cơ xảy ra lỗi phân tích cú pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 3 4 5`

`2 4 5 1`| Bước | Alice Sum | Bob Sum | Sự khác biệt | 
| --- | --- | --- | --- | 
| Sau khi đọc đầu vào | 15 | 12 | 3 | 

Sự khác biệt là`3`, khớp với giá trị còn thiếu. Điều này xác nhận rằng tất cả các phần tử được chia sẻ sẽ bị hủy hoàn toàn bất kể thứ tự. 

### Ví dụ 2 

đầu vào:`10 14 22 32 110`

`14 22 32 110`| Bước | Alice Sum | Bob Sum | Sự khác biệt | 
| --- | --- | --- | --- | 
| Sau khi đọc đầu vào | 188 | 178 | 10 | 

Kết quả là`10`, đó chính xác là giá trị không có trong danh sách của Bob. Ví dụ này chứng tỏ rằng phương thức này không dựa vào thứ tự hoặc sự kề cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các danh sách có kích thước cố định có độ dài 5 và 4 mới được xử lý | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung ngoài một vài biến | 

Việc tính toán là công việc liên tục nên dễ dàng thỏa mãn các ràng buộc ngay cả khi thực hiện lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

def solve():
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    return str(sum(a) - sum(b))

# provided samples
assert run("1 2 3 4 5\n2 4 5 1\n") == "3", "sample 1"
assert run("10 14 22 32 110\n14 22 32 110\n") == "10", "sample 2"

# custom cases
assert run("1 1 1 1 1\n1 1 1 1\n") == "1", "all equal values"
assert run("100 2 3 4 5\n2 3 4 5\n") == "100", "missing largest"
assert run("1 2 3 4 5\n1 2 3 4\n") == "5", "missing last element"
assert run("7 8 9 10 11\n8 9 10 11\n") == "7", "missing first element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các giá trị bằng nhau | 1 | việc hủy bỏ vẫn cô lập giá trị lặp lại | 
| Thiếu lớn nhất | 100 | hoạt động khi câu trả lời là phần tử tối đa | 
| Thiếu phần tử cuối cùng | 5 | độc lập về vị trí | 
| Thiếu phần tử đầu tiên | 7 | không có giả định đặt hàng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị ngoại trừ giá trị bị thiếu đều giống hệt nhau. Đối với đầu vào như`1 1 1 1 1`Và`1 1 1 1`, phép trừ vẫn cô lập phần còn thiếu`1`chính xác vì bốn bản sao bị hủy và một bản còn lại. 

Một trường hợp khác là khi giá trị còn thiếu là số nhỏ nhất hoặc lớn nhất. Vì`100 2 3 4 5`với Bob có`2 3 4 5`, tổng chênh lệch tạo ra`100`trực tiếp vì các thái cực không ảnh hưởng đến hành vi hủy bỏ. 

Trường hợp cuối cùng là sắp xếp tùy ý, chẳng hạn như`7 8 9 10 11`Và`8 9 10 11`. Mặc dù giá trị bị thiếu nằm ở đầu đầu vào, cấu trúc vị trí không liên quan vì chỉ tổng hợp mới quan trọng và phép trừ vẫn mang lại kết quả`7`không phụ thuộc vào sự sắp xếp.
