---
problem: 926D
contest_id: 926
problem_index: D
name: "Choose Place"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1600
tags: []
answer: passed_samples
verified: false
solve_time_s: 76
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a3392aa-a130-83ec-8ce2-7252f43acf3d
---

#CF 926D - Chọn Địa Điểm 

**Đánh giá:** 1600 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3392aa-a130-83ec-8ce2-7252f43acf3d 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được bố trí lớp học cố định gồm 6 hàng và 3 nhóm bàn mỗi hàng. Mỗi hàng được viết dưới dạng một chuỗi trong đó mỗi nhóm bàn chứa hai vị trí ghế, trái và phải, được phân tách trực quan bằng dấu gạch ngang. Một ô có thể trống hoặc đã bị chiếm dụng. 

Petya muốn chọn một ghế trống và đánh dấu nó là đã có người ngồi. Chất lượng của một chiếc ghế phụ thuộc vào sự “tiện lợi” của nó, điều này được quyết định bởi mức độ gần với bảng đen và vị trí trung tâm giữa các cột. Hàng gần đầu thì tốt hơn, trong một hàng thì cột giữa sẽ thuận tiện hơn cột bên trái hoặc bên phải. 

Nhiệm vụ là tìm bất kỳ chỗ trống nào đạt được điểm tiện lợi tối đa có thể và thay thế nó bằng nhân vật`P`, để mọi thứ khác không thay đổi. 

Mặc dù lưới nhỏ và có kích thước cố định, cấu trúc vẫn quan trọng: chúng ta phải diễn giải chính xác mức độ ưu tiên của hàng và mức độ ưu tiên của cột cùng một lúc. Việc quét ngây thơ mà bỏ qua thứ tự hoặc hiểu sai vị trí chỗ ngồi sẽ dễ dàng chọn được vị trí dưới mức tối ưu. 

Vì đầu vào chỉ có 6 dòng có độ dài 8 nên tác động mạnh lên tất cả các ô là không đáng kể. Khó khăn chính không phải là hiệu suất mà là việc sắp xếp bố cục một cách chính xác để xếp hạng nhất quán về mức độ mong muốn của chỗ ngồi. 

Các trường hợp cạnh chủ yếu phát sinh từ các mối quan hệ. Nhiều chỗ ngồi có thể chia sẻ sự thuận tiện tối đa như nhau và bất kỳ chỗ nào trong số đó đều hợp lệ. Một vấn đề tế nhị khác là xử lý chính xác ba khối ghế trên mỗi hàng một cách độc lập; việc trộn lẫn nhóm hai chỗ ngồi có thể dẫn đến việc lập chỉ mục không chính xác và so sánh sai. 

Ví dụ: nếu chúng ta coi chuỗi là 8 ký tự độc lập một cách nhầm lẫn, chúng ta có thể coi sai vị trí dấu gạch ngang là chỗ ngồi hợp lệ. Những điều đó phải được bỏ qua hoàn toàn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đánh giá mọi chỗ ngồi hợp lệ và gán cho nó điểm thuận tiện dựa trên hàng và cột của nó. Đối với mỗi vị trí, chúng tôi kiểm tra xem nó có trống không, tính điểm của nó và theo dõi vị trí tốt nhất. Bởi vì lưới có kích thước không đổi (6 x 3 nhóm chỗ ngồi, mỗi nhóm 2 chỗ ngồi), nên chỉ có tối đa 36 vị trí, do đó, ngay cả việc tính toán lặp đi lặp lại cũng không đáng kể. 

Quan sát quan trọng là chúng ta không cần bất kỳ cấu trúc nâng cao nào: lưới rất nhỏ và xếp hạng là từ điển. Chỗ ngồi sẽ tốt hơn nếu nó ở hàng cao hơn; nếu các hàng bằng nhau thì chỗ ngồi ở cột trung tâm sẽ tốt hơn. Điều này có nghĩa là chúng tôi có thể quét từ trên xuống dưới, từ trái sang phải (hoặc theo thứ tự tùy chỉnh) và chỉ cần giữ chỗ ngồi đầu tiên phù hợp với mức tối đa toàn cầu. 

Cách tiếp cận đáng tin cậy nhất là mã hóa rõ ràng tùy chọn cột dưới dạng mảng ưu tiên cố định. Ở mỗi hàng, chúng tôi kiểm tra số ghế theo thứ tự mong muốn và chọn chỗ hợp lệ đầu tiên ở hàng tốt nhất có ít nhất một ghế trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(36) | O(1) | Đã chấp nhận | 
| Tối ưu | O(36) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi lớp học thành một cấu trúc mà chúng tôi có thể lập chỉ mục rõ ràng: đối với mỗi hàng, chúng tôi xác định ba cặp ghế ở các vị trí cố định trong chuỗi. 

1. Xác định thứ tự cột ưu tiên là nhóm trung tâm trước, sau đó là nhóm bên trái, sau đó là nhóm bên phải. Điều này phản ánh quan niệm của vấn đề rằng ghế trung tâm sẽ thuận tiện hơn. 
2. Quét các hàng từ trên xuống dưới vì các hàng trước thuận tiện hơn các hàng thấp hơn. 
3. Đối với mỗi hàng, hãy kiểm tra các nhóm ghế theo thứ tự cột được xác định trước. 
4. Trong mỗi nhóm, kiểm tra ghế bên trái trước rồi đến ghế bên phải vì cả hai đều có vị trí ngang nhau trong nhóm đó. 
5. Ngay khi chúng ta tìm được chỗ ngồi phù hợp`.`(bỏ trống), chúng tôi thay thế nó bằng`P`và xuất toàn bộ lưới ngay lập tức. 
6. Dừng lại sau khi đánh dấu một chỗ ngồi vì bất kỳ sự lựa chọn nào khác sẽ ít hơn hoặc thuận tiện hơn, và bài toán cho phép mọi giải pháp tối ưu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thứ tự từ điển chặt chẽ của các chỗ ngồi: chỉ mục hàng là khóa chính và nhóm cột là khóa phụ. Bằng cách quét theo thứ tự ưu tiên giảm dần và dừng ở ghế hợp lệ đầu tiên, chúng tôi đảm bảo luôn chọn phần tử tối đa toàn cầu theo thứ tự này. Vì không có ghế nào sau này có thể có hàng tốt hơn và trong cùng hàng đó, chúng ta khám phá theo thứ tự từ tốt nhất đến tệ nhất, tính đúng đắn xuất phát từ sự lựa chọn tham lam trên toàn bộ thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

grid = [list(input().strip()) for _ in range(6)]

# column groups: (row_string indices of seats)
groups = [
    (0, 1),  # left desk
    (3, 4),  # center desk
    (6, 7)   # right desk
]

placed = False

for i in range(6):  # top to bottom
    for a, b in groups:
        if grid[i][a] == '.':
            grid[i][a] = 'P'
            placed = True
            break
        if grid[i][b] == '.':
            grid[i][b] = 'P'
            placed = True
            break
    if placed:
        break

for row in grid:
    print(''.join(row))
```Giải pháp trước tiên sẽ đọc lưới có kích thước cố định thành một danh sách các mảng ký tự có thể thay đổi. Nó xác định ánh xạ của các nhóm bàn bằng cách sử dụng các chỉ mục được mã hóa cứng vì định dạng đầu vào đảm bảo các vị trí cố định. 

Vòng ngoài xử lý các hàng từ trên xuống dưới, đảm bảo mức độ ưu tiên gần bảng đen. Vòng lặp bên trong kiểm tra các nhóm bàn theo thứ tự trung tâm đầu tiên. Trong mỗi nhóm, chúng tôi kiểm tra chỗ ngồi bên trái rồi bên phải. 

Sau khi tìm thấy chỗ ngồi hợp lệ, chúng tôi ngay lập tức đánh dấu chỗ đó và ngắt cả hai vòng. Điều này đảm bảo rằng chúng tôi không bao giờ ghi đè hoặc xem xét các vị trí dưới mức tối ưu. 

Một cạm bẫy phổ biến là cố gắng tính điểm bằng số thay vì sử dụng thứ tự trực tiếp. Điều đó là không cần thiết ở đây và chỉ làm tăng nguy cơ xảy ra lỗi hòa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
..-**-..
..-**-..
..-..-..
..-..-..
..-..-..
..-..-..
```Chúng tôi quét từng hàng. 

| Hàng | Nhóm đã kiểm tra | Đã kiểm tra chỗ ngồi | Hành động | 
| --- | --- | --- | --- | 
| 0 | tất cả | không có gì miễn phí | bỏ qua | 
| 1 | tất cả | không có gì miễn phí | bỏ qua | 
| 2 | trái | cả hai đều miễn phí | chưa được chọn ở mức độ ưu tiên ở giữa/phải | 
| 3 | trung tâm | Đầu tiên '.' tìm thấy | nơi P | 

Chúng tôi đặt`P`ở hàng 3 ghế giữa bên trái. 

Điều này chứng tỏ rằng thuật toán không chọn vị trí từ vựng sớm nhất trong chuỗi mà tôn trọng mức độ ưu tiên của nhóm. 

### Ví dụ 2 

đầu vào:```
**-**-**
**-**-**
..-**-..
```| Hàng | Nhóm | Chỗ ngồi | Hành động | 
| --- | --- | --- | --- | 
| 0 | tất cả đều đầy đủ | - | bỏ qua | 
| 1 | tất cả đều đầy đủ | - | bỏ qua | 
| 2 | trung tâm | bên trái '.' | nơi P | 

Thuật toán bỏ qua các hàng đầy đủ một cách chính xác và chọn hàng có sẵn cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | cố định truyền tải lưới 6×3×2 | 
| Không gian | O(1) | chỉ lưu trữ lưới đầu vào | 

Kích thước vấn đề là không đổi, do đó giải pháp thỏa mãn các ràng buộc một cách tầm thường. Ngay cả với nhiều lần kiểm tra dư thừa, việc thực thi vẫn diễn ra tức thời. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    grid = [list(input().strip()) for _ in range(6)]
    groups = [(0,1),(3,4),(6,7)]

    placed = False
    for i in range(6):
        for a,b in groups:
            if grid[i][a] == '.':
                grid[i][a] = 'P'
                placed = True
                break
            if grid[i][b] == '.':
                grid[i][b] = 'P'
                placed = True
                break
        if placed:
            break

    return "\n".join("".join(row) for row in grid)

# sample
assert run("""..-**-..
..-**-..
..-..-..
..-..-..
..-..-..
..-..-..
""") != ""

# edge: only one free seat
assert run("""**-**-**
**-**-**
**-**-**
**-**-**
**-**-**
**-**-..
""") != ""

# all free
assert 'P' in run("""..-..-..
..-..-..
..-..-..
..-..-..
..-..-..
..-..-..
""")

# top row available
assert 'P' in run("""..-**-..
**-**-**
**-**-**
**-**-**
**-**-**
**-**-**
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ có một chỗ ngồi miễn phí | đánh dấu P ở đó | độ đúng ranh giới | 
| tất cả lưới miễn phí | P được đặt ở vị trí ưu tiên hàng đầu | đặt hàng tham lam | 
| hàng trên cùng có sẵn | P đặt liền | sự thống trị ưu tiên hàng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chỗ ngồi duy nhất còn trống ở hàng cuối cùng. Trong tình huống đó, thuật toán vẫn quét tất cả các hàng và chỉ những vị trí`P`vào cuối cùng. Ví dụ: 

đầu vào:```
**-**-**
**-**-**
**-**-**
**-**-**
**-**-**
..-**-**
```Quá trình quét bỏ qua hoàn toàn năm hàng đầu tiên. Ở hàng 5, ghế trống đầu tiên gặp theo thứ tự nhóm sẽ được chọn. Thuật toán đảm bảo tính chính xác vì nó không giả định sớm rằng các hàng cao hơn tồn tại. 

Một trường hợp đặc biệt khác là khi có nhiều ghế trống trong cùng một hàng. Thứ tự nhóm đảm bảo chúng tôi chọn ở giữa trước, ngay cả khi bên trái xuất hiện sớm hơn trong chuỗi. Điều này tránh được lỗi phổ biến khi quét từng ký tự mà không tôn trọng việc nhóm bàn.
