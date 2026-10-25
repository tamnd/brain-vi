---
title: "CF 106073J - Jo\u00e3o Jo\u00e3o"
description: "Chúng ta được cung cấp một tập hợp cố định gồm 10 nhiệm vụ, mỗi nhiệm vụ được gắn nhãn với giá trị độ khó từ tập hợp {1, 2, 3, 4}. Người tổ chức muốn cuối cùng có thể chọn một bộ bốn nhiệm vụ, mỗi nhiệm vụ có độ khó khác nhau."
date: "2026-06-21T09:26:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106073
codeforces_index: "J"
codeforces_contest_name: "The 2025 ICPC South America - Brazil First Phase"
rating: 0
weight: 106073
solve_time_s: 41
verified: true
draft: false
---

[CF 106073J - Jo\u00e3o Jo\u00e3o](https://codeforces.com/problemset/problem/106073/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp cố định gồm 10 nhiệm vụ, mỗi nhiệm vụ được gắn nhãn với giá trị độ khó từ tập hợp {1, 2, 3, 4}. Người tổ chức muốn cuối cùng có thể chọn một bộ bốn nhiệm vụ, mỗi nhiệm vụ có độ khó khác nhau. Hiện tại, một số khó khăn có thể đã xuất hiện trong số 10 nhiệm vụ và một số có thể bị thiếu hoàn toàn. 

Nhiệm vụ là xác định số lượng nhiệm vụ bổ sung tối thiểu phải được tạo để tất cả bốn mức độ khó đều xuất hiện ít nhất một lần trong nhóm nhiệm vụ. 

Đầu vào chỉ đơn giản là 10 số nguyên, mỗi số mô tả độ khó của một nhiệm vụ hiện có. Đầu ra là một số nguyên: có bao nhiêu trong số bốn mức độ khó hiện chưa có. 

Vì kích thước đầu vào không đổi (chính xác là 10 số), nên không có ràng buộc tiệm cận có ý nghĩa nào. Bất kỳ giải pháp nào, thậm chí là công việc làm thêm tuyến tính hoặc liên tục, đều đủ. Điều quan trọng là tính đúng đắn hơn là hiệu quả. 

Dạng lỗi chính theo cách tiếp cận đơn giản là nhầm lẫn tần số với sự hiện diện. Chúng tôi không quan tâm có bao nhiêu nhiệm vụ có độ khó vượt quá 0 so với khác 0. Ví dụ: có mười nhiệm vụ ở độ khó 1 sẽ không giúp ích gì nếu thiếu độ khó 2, 3 và 4. Một giải pháp đúng chỉ phải theo dõi xem mỗi khó khăn có xuất hiện ít nhất một lần hay không. 

Trường hợp khó khăn thứ hai phát sinh khi cả bốn khó khăn đều đã xuất hiện. Trong trường hợp đó, không có nhiệm vụ mới nào được yêu cầu và câu trả lời là không. Bất kỳ cách tiếp cận nào tính toán “số trừ 4” đều phải đảm bảo nó không bao giờ tạo ra giá trị âm. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng việc xây dựng nhóm khó khăn còn thiếu bằng cách liên tục thêm các nhiệm vụ cho đến khi tất cả bốn loại khó khăn đều xuất hiện. Vì mỗi phần bổ sung chỉ có thể giới thiệu một danh mục còn thiếu, nên người ta có thể liên tục kiểm tra xem những khó khăn nào còn thiếu và thêm từng khó khăn đó về mặt khái niệm. 

Điều này hoạt động nhưng là gián tiếp không cần thiết. Trong trường hợp xấu nhất, người ta sẽ quét tập hiện tại sau mỗi phép cộng giả định, dẫn đến công việc lặp đi lặp lại mặc dù không gian trạng thái cực kỳ nhỏ và tĩnh. 

Quan sát quan trọng là yêu cầu cuối cùng chỉ phụ thuộc vào giá trị nào trong bốn giá trị xuất hiện ít nhất một lần. Mỗi loại độ khó là độc lập, vì vậy chúng ta có thể nén toàn bộ dữ liệu đầu vào thành một mảng hiện diện boolean có kích thước 4. Khi đã biết điều này, câu trả lời chỉ đơn giản là số lượng danh mục còn thiếu. 

Điều này làm giảm vấn đề từ quá trình “xây dựng” động sang vấn đề đếm tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(10 * 4) khái niệm | O(4) | Được chấp nhận nhưng không cần thiết | 
| Đếm sự hiện diện | O(10) | O(4) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cấu trúc boolean`seen[1..4]`với tất cả các giá trị được đặt thành sai. Điều này thể hiện liệu chúng ta có gặp phải từng cấp độ khó cho đến nay hay không. 
2. Đọc lần lượt 10 giá trị đầu vào. Với mỗi giá trị`d`, đánh dấu`seen[d] = true`. Bước này nén toàn bộ tập dữ liệu vào một bản đồ tồn tại đơn giản thay vì lưu trữ tần số. 
3. Sau khi xử lý tất cả các giá trị, lặp lại bốn mức độ khó và đếm xem có bao nhiêu mục trong`seen`vẫn là sai sự thật. Mỗi mục sai tương ứng với một loại độ khó còn thiếu. 
4. Xuất trực tiếp số đếm này, vì mỗi độ khó còn thiếu yêu cầu đưa ra chính xác một nhiệm vụ mới. 

Thuật toán tránh mọi mô phỏng tạo nhiệm vụ vì mỗi danh mục bị thiếu là độc lập và yêu cầu chính xác một bổ sung bất kể có bao nhiêu nhiệm vụ đã tồn tại trong các danh mục khác. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là yêu cầu này hoàn toàn mang tính tồn tại: chúng ta chỉ cần ít nhất một nhiệm vụ ở mỗi cấp độ khó. Đầu vào góp phần đáp ứng một mức độ theo cách nhị phân, nó tồn tại hoặc không. Do đó, việc nén tất cả các lần xuất hiện vào trạng thái hiện diện boolean sẽ bảo toàn tất cả thông tin liên quan đến câu trả lời cuối cùng. Số lượng cuối cùng của các danh mục bị thiếu trực tiếp bằng số lượng nhiệm vụ mới cần thiết, vì mỗi lần bổ sung có thể đưa ra chính xác một độ khó còn thiếu trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

vals = list(map(int, input().split()))

seen = [False] * 5

for v in vals:
    seen[v] = True

ans = 0
for d in range(1, 5):
    if not seen[d]:
        ans += 1

print(ans)
```Lời giải đọc 10 số nguyên và ngay lập tức đánh dấu mức độ khó nào xuất hiện. Mảng`seen`có kích thước 5 để các chỉ số căn chỉnh tự nhiên với các nhãn độ khó từ 1 đến 4, tránh nhầm lẫn. 

Vòng lặp cuối cùng chỉ đơn giản là đếm xem có bao nhiêu vị trí trong số bốn vị trí còn sai. Điều này tránh mọi thủ thuật số học như phép trừ từ 4, có thể dễ xảy ra lỗi nếu không được giới hạn cẩn thận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3 4 1 3 4 1 3 4 1
```Chúng tôi theo dõi sự hiện diện như sau: 

| Bước | Giá trị | đã thấy[1] | đã thấy[2] | đã thấy[3] | đã thấy[4] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | đúng | sai | sai | sai | 
| 2 | 3 | đúng | sai | đúng | sai | 
| 3 | 4 | đúng | sai | đúng | đúng | 
| 4-10 | lặp lại | đúng | sai | đúng | đúng | 

Sau khi xử lý chỉ còn thiếu độ khó 2. Thuật toán đếm một mục nhập và đầu ra bị thiếu 1. Điều này xác nhận rằng các lần xuất hiện trùng lặp không ảnh hưởng đến tính chính xác, vì sự hiện diện là tất cả những gì quan trọng. 

### Ví dụ 2 

đầu vào:```
4 1 1 4 3 1 2 1 2 2
```| Bước | Giá trị | đã thấy[1] | đã thấy[2] | đã thấy[3] | đã thấy[4] | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi xử lý tất cả | hỗn hợp | đúng | đúng | đúng | đúng | 

Tất cả bốn cấp độ khó đều có mặt nên không cần thực hiện nhiệm vụ mới. Thuật toán xuất ra 0, xác nhận rằng logic xử lý chính xác trường hợp hoàn toàn hài lòng mà không tạo ra giá trị âm hoặc không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10) | Chúng tôi quét đầu vào có kích thước cố định một lần và thực hiện cập nhật liên tục theo thời gian cho mỗi phần tử | 
| Không gian | O(1) | Chỉ một mảng boolean cố định có kích thước 4 (cộng với bộ nhớ đầu vào) | 

Các ràng buộc có kích thước không đổi nên giải pháp này chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. Ngay cả dưới những ràng buộc tổng quát, cách tiếp cận tương tự cũng có tỷ lệ tuyến tính với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    vals = list(map(int, input().split()))
    seen = [False] * 5
    for v in vals:
        seen[v] = True
    ans = sum(1 for d in range(1, 5) if not seen[d])
    return str(ans)

# provided samples
assert run("1 3 4 1 3 4 1 3 4 1") == "1"
assert run("4 1 1 4 3 1 2 1 2 2") == "0"

# all same value
assert run("1 1 1 1 1 1 1 1 1 1") == "3"

# already complete set
assert run("1 2 3 4 1 2 3 4 1 2") == "0"

# missing two categories
assert run("1 1 1 1 1 1 1 1 1 3") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả 1s | 3 | sự đa dạng còn thiếu tồi tệ nhất | 
| trọn bộ | 0 | không cần bổ sung | 
| chỉ 1 và 3 giây | 2 | bảo hiểm một phần | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các nhiệm vụ thuộc về một độ khó duy nhất. Ví dụ: 

đầu vào:```
1 1 1 1 1 1 1 1 1 1
```Thuật toán chỉ đánh dấu`seen[1] = true`. Ba giá trị còn lại là sai nên tổng số là 3. Điều này phù hợp với yêu cầu là chúng ta phải đưa ra một nhiệm vụ cho mỗi độ khó 2, 3 và 4. 

Một trường hợp đặc biệt khác là khi đầu vào đã chứa tất cả các khó khăn nhưng theo thứ tự rải rác:```
2 3 1 4 2 3 1 4 2 3
```Sau khi xử lý xong, tất cả`seen`các mục trở thành đúng sự thật. Vòng lặp đếm 0 danh mục bị thiếu, tạo ra kết quả 0. Độ chính xác đến từ việc coi các lần xuất hiện lặp lại là không liên quan, vì sự hiện diện đã bão hòa sau lần xuất hiện đầu tiên của mỗi giá trị.
