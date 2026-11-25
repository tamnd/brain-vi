---
problem: 987A
contest_id: 987
problem_index: A
name: "Infinity Gauntlet"
contest_name: "Codeforces Round 485 (Div. 2)"
rating: 800
tags: ["implementation"]
answer: passed_samples
verified: false
solve_time_s: 76
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33a990-3914-83ec-a201-26b6a39a5516
---

#CF 987A - Găng Tay Vô Cực 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a990-3914-83ec-a201-26b6a39a5516 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một bộ đá màu nhỏ được đặt vào một cấu trúc cố định: Găng tay Vô cực luôn có chính xác sáu khe có thể. Mỗi ô tương ứng với một Viên ngọc Vô cực duy nhất và mỗi viên ngọc được xác định duy nhất bằng màu sắc của nó. 

Dữ liệu đầu vào cho chúng ta biết Thanos hiện có bao nhiêu viên đá quý, sau đó liệt kê màu sắc của những viên đá quý đó. Từ thông tin một phần này, chúng ta phải xác định viên đá quý nào bị thiếu bằng cách so sánh với bộ sáu màu có thể cố định đầy đủ. Đầu ra là số lượng đá quý bị thiếu theo sau là tên của chúng. 

Chi tiết cấu trúc quan trọng là vũ trụ của các khả năng là không đổi và rất nhỏ. Luôn có chính xác sáu màu và tất cả các màu đầu vào đều khác biệt. Điều này ngay lập tức loại bỏ mọi nhu cầu tìm kiếm hoặc tối ưu hóa phức tạp. Nhiệm vụ giảm xuống để đặt phép trừ giữa hai bộ cố định. 

Ràng buộc trên n nhiều nhất là 6, có nghĩa là ngay cả cách tiếp cận ngây thơ nhất là kiểm tra tư cách thành viên nhiều lần cũng nhanh chóng một cách tầm thường. Bất kỳ giải pháp nào lặp qua danh sách không đổi hoặc sử dụng một tập hợp sẽ chạy trong thời gian không đổi. 

Các trường hợp cạnh duy nhất đến từ các giá trị biên của n. Khi n bằng 0, chúng ta phải xuất ra tất cả sáu viên ngọc. Khi n là 6, chúng tôi không xuất ra. Một trường hợp thất bại nhỏ trong việc triển khai bất cẩn là quên duy trì ánh xạ chính xác giữa tên màu và tên đá quý hoặc viết hoa không khớp vì định dạng đầu ra yêu cầu tên đá quý được viết hoa trong khi màu đầu vào là chuỗi chữ thường. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi đây là vấn đề thành viên chung. Chúng ta có thể lưu trữ các màu đầu vào trong một danh sách và đối với mỗi màu trong số sáu màu có thể, hãy quét danh sách để kiểm tra xem nó có tồn tại hay không. Mỗi lần kiểm tra thành viên có chi phí O(n) và chúng tôi thực hiện sáu lần kiểm tra không đổi, dẫn đến O(6n), giá trị này vẫn không đổi trong bài toán này. 

Tuy nhiên, ngay cả điều này cũng là chi phí không cần thiết. Cấu trúc của bài toán gợi ý sự ánh xạ trực tiếp giữa màu sắc và tên đá quý. Vì vũ trụ là cố định nên chúng ta có thể chỉ cần lưu trữ tất cả sáu cặp trong từ điển hoặc danh sách, sau đó loại bỏ những cặp xuất hiện trong đầu vào. 

Quan sát chính là chúng tôi không giải quyết được vấn đề truy vấn động. Chúng tôi không được yêu cầu xử lý các bản cập nhật hoặc nhiều trường hợp thử nghiệm có độ biến thiên lớn. Chúng tôi chỉ đơn giản là lọc một danh mục cố định. Điều này làm cho sự khác biệt được thiết lập trực tiếp trở thành cách tiếp cận rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Ánh xạ tập hợp trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách duy trì ánh xạ cố định giữa màu sắc và tên đá quý, sau đó loại bỏ những màu xuất hiện trong đầu vào. 

1. Xây dựng bảng tham chiếu gồm tất cả sáu viên đá quý, ghép từng màu với tên đá quý tương ứng. Điều này thể hiện tập hợp đầy đủ trước bất kỳ quan sát nào. 
2. Đọc tất cả các màu đầu vào và lưu chúng vào một bộ. Việc sử dụng một bộ là không thực sự cần thiết nhưng nó đảm bảo kiểm tra tư cách thành viên nhanh chóng và tránh trùng lặp ngẫu nhiên nếu giả định đầu vào bị vi phạm. 
3. Lặp lại danh sách đầy đủ sáu màu có thể. 
4. Đối với mỗi màu, hãy kiểm tra xem nó có xuất hiện trong bộ đầu vào hay không. 
5. Nếu nó không xuất hiện, hãy thêm tên đá quý tương ứng vào danh sách câu trả lời. 
6. Sau khi xử lý tất cả sáu viên, xuất ra số lượng viên ngọc bị thiếu theo sau là tên của chúng. 

Lý do chúng tôi lặp lại toàn bộ danh sách cố định thay vì lặp lại đầu vào là vì chúng tôi được yêu cầu tìm các phần tử bị thiếu chứ không phải các phần tử hiện có. Hướng lặp lại này đảm bảo chúng ta không bao giờ quên xem xét những viên ngọc vắng mặt. 

### Tại sao nó hoạt động

Thuật toán dựa trên tính bất biến rằng mỗi viên đá quý hợp lệ được liên kết duy nhất với chính xác một màu và vũ trụ các màu có thể có là cố định và đầy đủ. Ở mỗi bước, chúng tôi phân loại từng viên ngọc có thể có vào đúng một trong hai trạng thái: được quan sát hoặc không được quan sát. Vì đầu vào không chứa bản sao và chỉ có màu hợp lệ nên không có sự mơ hồ nào phát sinh. Do đó, mọi viên ngọc bị thiếu phải chính xác là viên có màu không xuất hiện trong tập đầu vào, đảm bảo tính chính xác của logic trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    color_to_name = {
        "purple": "Power",
        "green": "Time",
        "blue": "Space",
        "orange": "Soul",
        "red": "Reality",
        "yellow": "Mind"
    }
    
    present = set()
    for _ in range(n):
        present.add(input().strip())
    
    missing = []
    for color, name in color_to_name.items():
        if color not in present:
            missing.append(name)
    
    print(len(missing))
    for x in missing:
        print(x)

if __name__ == "__main__":
    solve()
```Giải pháp xoay quanh từ điển`color_to_name`, mã hóa sự phân đôi cố định giữa màu đầu vào và tên đá quý đầu ra. Điều này tránh mọi logic có điều kiện hoặc ánh xạ thủ công bên trong các vòng lặp. 

Chúng tôi lưu trữ các màu đầu vào trong một bộ để đảm bảo việc kiểm tra tư cách thành viên liên tục. Mặc dù n nhiều nhất là 6 nhưng điều này làm cho logic rõ ràng và mạnh mẽ. 

Việc lặp lại từ điển đảm bảo chúng tôi luôn kiểm tra tất cả các loại đá quý có thể có chính xác một lần, bất kể kích thước đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
red
purple
yellow
orange
```Chúng tôi theo dõi quá trình: 

| Bước | Đã kiểm tra màu | Trong Bộ đầu vào | Hành động | Danh sách Thiếu | 
| --- | --- | --- | --- | --- | 
| 1 | màu tím | vâng | bỏ qua | [] | 
| 2 | màu xanh lá cây | không | thêm thời gian | ["Thời gian"] | 
| 3 | màu xanh | không | thêm không gian | ["Thời gian", "Không gian"] | 
| 4 | cam | vâng | bỏ qua | ["Thời gian", "Không gian"] | 
| 5 | đỏ | vâng | bỏ qua | ["Thời gian", "Không gian"] | 
| 6 | màu vàng | vâng | bỏ qua | ["Thời gian", "Không gian"] | 

Đầu ra:```
2
Time
Space
```Dấu vết này cho thấy thuật toán không nhạy cảm với thứ tự đầu vào và chỉ tích lũy chính xác các phần tử vắng mặt. 

### Ví dụ 2 

đầu vào:```
0
```| Bước | Đã kiểm tra màu | Trong Bộ đầu vào | Hành động | Danh sách Thiếu | 
| --- | --- | --- | --- | --- | 
| 1 | màu tím | không | thêm sức mạnh | ["Quyền lực"] | 
| 2 | màu xanh lá cây | không | thêm thời gian | ["Sức mạnh", "Thời gian"] | 
| 3 | màu xanh | không | thêm không gian | ["Sức mạnh", "Thời gian", "Không gian"] | 
| 4 | cam | không | thêm Linh hồn | ["Sức mạnh", "Thời gian", "Không gian", "Linh hồn"] | 
| 5 | đỏ | không | thêm thực tế | ["Sức mạnh", "Thời gian", "Không gian", "Linh hồn", "Hiện thực"] | 
| 6 | màu vàng | không | thêm Tâm | ["Sức mạnh", "Thời gian", "Không gian", "Linh hồn", "Hiện thực", "Tâm trí"] | 

Đầu ra:```
6
Power
Time
Space
Soul
Reality
Mind
```Trường hợp này xác nhận rằng thuật toán xử lý chính xác tập đầu vào trống và trả về toàn bộ vũ trụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chúng tôi luôn xử lý chính xác sáu mục cố định bất kể kích thước đầu vào | 
| Không gian | O(1) | Chỉ sử dụng từ điển và bộ có kích thước không đổi | 

Việc tính toán được giới hạn bởi một vũ trụ không đổi gồm sáu phần tử, do đó nó thỏa mãn mọi ràng buộc một cách tầm thường. Việc sử dụng bộ nhớ cũng cố định và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    color_to_name = {
        "purple": "Power",
        "green": "Time",
        "blue": "Space",
        "orange": "Soul",
        "red": "Reality",
        "yellow": "Mind"
    }

    n = int(input().strip())
    present = set(input().strip() for _ in range(n))

    missing = [name for color, name in color_to_name.items() if color not in present]

    out = [str(len(missing))] + missing
    return "\n".join(out)

# provided sample
assert run("4\nred\npurple\nyellow\norange\n") == "2\nTime\nSpace"

# all missing
assert run("0\n") == "6\nPower\nTime\nSpace\nSoul\nReality\nMind"

# all present
assert run("6\npurple\ngreen\nblue\norange\nred\nyellow\n") == "0"

# single element
assert run("1\nblue\n") == "5\nPower\nTime\nSoul\nReality\nMind"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 đá quý | tất cả sáu viên ngọc | trường hợp vắng mặt đầy đủ | 
| 6 viên đá quý | 0 | trường hợp có mặt đầy đủ | 
| 1 màu xanh | 5 thiếu | tính chính xác của việc lọc một phần | 
| trường hợp mẫu | 2 Không Gian Thời Gian | sự đúng đắn của thành viên hỗn hợp | 

## Vỏ cạnh 

Đối với trường hợp đầu vào trống`n = 0`, thuật toán khởi tạo tập đầu vào là trống. Mỗi lần lặp lại danh sách sáu màu cố định đều không kiểm tra được tư cách thành viên, vì vậy tất cả sáu tên đá quý đều được thêm vào. Đầu ra chính xác sẽ trở thành bộ đá quý đầy đủ. 

Đối với trường hợp đầu vào đầy đủ`n = 6`, bộ chứa tất cả các màu. Mỗi lần tra cứu đều thành công nên không có viên ngọc nào được thêm vào danh sách còn thiếu. Thuật toán đưa ra số 0 và danh sách trống, khớp với định dạng được yêu cầu. 

Trong cả hai trường hợp, tính chính xác xuất phát từ thực tế là không gian lặp là cố định và không phụ thuộc vào kích thước đầu vào, do đó các giá trị biên chỉ ảnh hưởng đến việc kiểm tra thành viên chứ không ảnh hưởng đến cấu trúc luồng điều khiển.
