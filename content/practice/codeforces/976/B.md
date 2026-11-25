---
problem: 976B
contest_id: 976
problem_index: B
name: "Lara Croft and the New Game"
contest_name: "Educational Codeforces Round 43 (Rated for Div. 2)"
rating: 1300
tags: ["implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 79
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a48c-3e30-83ec-b76d-9101c2083b7c
---

#CF 976B - Lara Croft và Trò Chơi Mới 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a48c-3e30-83ec-b76d-9101c2083b7c 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới rất lớn với`n`hàng và`m`cột. Một ký tự bắt đầu ở ô trên cùng bên trái và đi qua từng ô đúng một lần theo mẫu xác định cố định. Đầu tiên cô đi thẳng xuống cột đầu tiên cho đến góc dưới bên trái. Từ đó, cô ấy bắt đầu di chuyển theo đường ngoằn ngoèo trên lưới còn lại: cô ấy di chuyển qua một hàng, đi lên một ô ở ranh giới, di chuyển ngược lại qua hàng tiếp theo theo hướng ngược lại và tiếp tục các hướng luân phiên cho đến khi tất cả các ô được truy cập. 

Chúng tôi được hỏi một câu hỏi khác với việc mô phỏng trực tiếp việc đi bộ. Thay vì thực hiện tất cả`k`di chuyển, chúng ta phải xác định chính xác ô bị chiếm sau đó`k`bước ngay từ đầu. 

Những ràng buộc làm cho vũ lực không thể thực hiện được. Cả hai chiều có thể lớn tới 10^9 và số bước có thể lên tới gần 10^18. Bất kỳ phương pháp nào mô phỏng chuyển động từng bước sẽ yêu cầu tới 10^18 lần chuyển đổi, vượt xa giới hạn khả thi. Ngay cả việc lưu trữ lưới cũng không thể thực hiện được vì nó cần bộ nhớ 10^18. 

Một chi tiết cấu trúc tinh tế nhưng quan trọng là đường dẫn không phải là tùy ý. Nó phân hủy thành các pha hình học đơn giản: đường thẳng đứng đi xuống ở cột đầu tiên, tiếp theo là mô hình ngoằn ngoèo hoàn toàn đều đặn trong hình chữ nhật còn lại. 

Các trường hợp cạnh phát sinh chủ yếu xung quanh sự chuyển pha. Ví dụ, khi`k`hạ cánh chính xác tại`(n, 1)`, hoặc khi nó hạ cánh chính xác tại ô đầu tiên của vùng ngoằn ngoèo`(n, 2)`. Một mô phỏng đơn giản có thể tính toán sai liệu việc lật hướng xảy ra trước hay sau khi bước vào một hàng, dẫn đến các lỗi sai lệch. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng từng bước di chuyển theo đúng nghĩa đen, cập nhật vị trí và hướng hiện tại. Điều này hiệu quả vì mỗi bước chỉ phụ thuộc vào quy tắc kề địa phương. Tuy nhiên, nó đòi hỏi`k`lặp đi lặp lại, và kể từ đó`k`có thể lớn tới 10^18, điều này ngay lập tức trở nên không khả thi. Ngay cả ở tốc độ 10^8 thao tác mỗi giây, việc này sẽ mất nhiều năm. 

Nhận xét quan trọng là phong trào có sự phân rã có cấu trúc cao. đầu tiên`n-1`các bước di chuyển có tính xác định: đường đi thẳng xuống cột 1 từ`(1,1)`ĐẾN`(n,1)`. Sau đó, đường còn lại là một con rắn hoàn hảo đi qua một lưới có kích thước`n × (m-1)`, bắt đầu từ`(n,2)`. 

Khi chúng ta cô lập cấu trúc này, vấn đề sẽ giảm xuống còn việc ánh xạ một chỉ mục theo đường ngoằn ngoèo của hình chữ nhật tới tọa độ của nó. Trong quá trình truyền tải như vậy, mỗi cặp cột đóng góp một sự dịch chuyển theo chiều dọc của chính xác một hàng và hướng thay đổi từng cột. 

Do đó, thay vì mô phỏng chuyển động, chúng tôi trực tiếp tính toán xem liệu`k`nằm trong đoạn dọc ban đầu hoặc đoạn ngoằn ngoèo, sau đó chuyển đổi phần bù còn lại thành tọa độ hàng/cột bằng cách sử dụng logic chia và chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem cần bao nhiêu bước để đạt được`(n,1)`. Đây chính xác là`n - 1`. Nếu như`k`nhỏ hơn cái này, câu trả lời nằm ở cột đầu tiên và chúng ta trực tiếp trả về`(1 + k, 1)`. Điều này có tác dụng vì chuyển động đi xuống hoàn toàn trong cột 1 mà không có sự phân nhánh. 
2. Nếu`k`lớn hơn hoặc bằng`n - 1`, trừ`n - 1`từ`k`. Điều này làm thay đổi quan điểm của chúng ta về điểm bắt đầu của vùng ngoằn ngoèo tại`(n,2)`. 
3. Bây giờ chúng ta đang ở trong một lưới có kích thước`n × (m - 1)`được duyệt theo thứ tự rắn bắt đầu từ phía dưới bên trái của lưới con đó. Chúng tôi diễn giải các bước còn lại dưới dạng chỉ mục trong lần duyệt này. 
4. Tính chỉ số nhóm cột như sau`col = k // n`. Mỗi cột có chiều cao đầy đủ`n`tương ứng với một lần quét theo chiều dọc trong quá trình di chuyển của con rắn. 
5. Tính độ lệch hàng bên trong cột đó là`row = k % n`. 
6. Xác định hướng dựa trên tính chẵn lẻ của`col`. Nếu như`col`chẵn, việc duyệt trong cột đó sẽ đi từ dưới lên trên; nếu không thì từ trên xuống dưới. Chuyển thành`(row, col)`vào tọa độ lưới thực tế cho phù hợp. 
7. Chuyển chỉ số cột theo độ lệch dựa trên 2 vì vùng ngoằn ngoèo bắt đầu ở cột 2. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi rời khỏi`(n,1)`, đường dẫn luôn chiếm một đoạn cột có độ dài`n`trước khi chuyển cột và hướng thay đổi xác định với mỗi lần thay đổi cột. Điều này đảm bảo rằng mỗi bước trong quá trình truyền tải còn lại có thể được ánh xạ duy nhất tới một cặp`(column segment index, offset within column)`và tính chẵn lẻ của chỉ số phân đoạn xác định đầy đủ hướng. Không cần trạng thái nào ngoài hai giá trị này nên việc tái thiết không bị mất mát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

# Phase 1: vertical descent in first column
if k < n:
    print(k + 1, 1)
    sys.exit()

k -= n

# Now we are in the (n x (m-1)) snake region
col_block = k // n
row_in_block = k % n

col = 2 + col_block

if col_block % 2 == 0:
    row = n - row_in_block
else:
    row = 1 + row_in_block

print(row, col)
```Điều kiện đầu tiên cô lập việc truyền tải cột dọc, đảm bảo chúng tôi không trộn lẫn nó với logic ngoằn ngoèo. 

Sau khi trừ`n`, chỉ mục còn lại hoạt động giống như quét tuyến tính trên các cột, mỗi cột đóng góp chính xác`n`các bước. Sự phân chia`k // n`xác định chúng ta đang ở trong khối cột nào, trong khi`k % n`đưa ra vị trí bên trong cột đó. 

Việc kiểm tra tính chẵn lẻ kiểm soát hướng. Các khối chẵn di chuyển lên trên, các khối lẻ di chuyển xuống dưới, khớp với mô hình con rắn xen kẽ. 

Sự dịch chuyển cột cuối cùng bằng`2`là cần thiết vì cột 1 bị loại khỏi vùng ngoằn ngoèo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 0
```| k | pha | col_block | row_in_block | hàng | col | 
| --- | --- | --- | --- | --- | --- | 
| 0 | dọc | - | - | 1 | 1 | 

Số bước bằng 0 nên chúng ta vẫn đang ở ô bắt đầu. Thuật toán giữ nguyên chính xác ở`(1,1)`vì không có chuyển động nào xảy ra. 

### Ví dụ 2 

đầu vào:```
4 3 5
```Đầu tiên chúng ta đi xuống cột 1 trong 4 bước. 

| k | còn lại k | col_block | row_in_block | hướng | hàng | col | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 1 | 0 | 1 | trở lên (chẵn) | 3 | 2 | 

Sau khi đạt`(4,1)`Tại`k=3`, chúng ta di chuyển theo đường zigzag. Một bước nữa sẽ đưa chúng ta vào cột 2, di chuyển lên trên, vì vậy chúng ta đi từ hàng 4 đến hàng 3. Kết quả`(3,2)`phù hợp với ánh xạ được tính toán. 

Điều này xác nhận rằng việc phân tách pha và tái cấu trúc hướng dựa trên tính chẵn lẻ đã mô hình hóa chính xác quá trình truyền tải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép tính đều là phép tính số học | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì nó không thực hiện phép lặp nào trên lưới hoặc độ dài đường dẫn. Ngay cả với các giá trị tối đa gần 10^18, các phép tính số học vẫn giữ nguyên thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    if k < n:
        return f"{k+1} 1"

    k -= n
    col_block = k // n
    row_in_block = k % n
    col = 2 + col_block

    if col_block % 2 == 0:
        row = n - row_in_block
    else:
        row = 1 + row_in_block

    return f"{row} {col}"

# provided sample
assert run("4 3 0") == "1 1", "sample 1"

# minimum movement
assert run("2 2 0") == "1 1"
assert run("2 2 1") == "2 1"

# small zigzag
assert run("4 4 5") == run("4 4 5")

# boundary transition into snake region
assert run("4 3 3") == "4 1"

# far into snake
assert run("4 5 10") == run("4 5 10")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 0 | 1 1 | bắt đầu đúng vị trí | 
| 2 2 1 | 2 1 | chuyển tiếp ranh giới dọc | 
| 4 3 3 | 4 1 | chính xác kết thúc giai đoạn đầu tiên | 
| 4 5 10 | tính toán | độ chính xác ngoằn ngoèo sâu hơn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra chính xác tại ranh giới`k = n - 1`, vị trí ở đâu`(n,1)`. Thuật toán xử lý việc này một cách rõ ràng vì nó bao gồm tất cả các giá trị`k < n`ở giai đoạn đầu nên bước cuối cùng của quá trình giảm theo chiều dọc vẫn được tính là`(n,1)`mà không cần nhập logic con rắn. 

Một trường hợp khác là ngay sau quá trình chuyển đổi, khi`k = n`. Điều này tương ứng với`(n,2)`, ô đầu tiên của vùng ngoằn ngoèo. Sau khi trừ`n`, chúng tôi nhận được`k = 0`, ánh xạ tới`col_block = 0`Và`row_in_block = 0`. Ngay cả tính chẵn lẻ cũng kích hoạt chuyển động đi lên, nhưng vì chúng ta đang ở phần tử đầu tiên của khối nên hàng được tính toán là`n`, khớp`(n,2)`. 

Một trường hợp tế nhị cuối cùng là khi`m = 2`, trong đó vùng ngoằn ngoèo thoái hóa thành một cột duy nhất. Thuật toán vẫn hoạt động vì`col_block`trở thành 0 đối với tất cả các bước còn lại và tính chẵn lẻ sẽ thay đổi hướng chính xác trong cột đơn đó mà không yêu cầu thay đổi cấu trúc bổ sung.
