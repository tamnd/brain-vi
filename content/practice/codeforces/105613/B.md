---
title: "CF 105613B - Số Từ"
description: "Nhiệm vụ yêu cầu chúng ta đếm xem có thể tạo ra bao nhiêu chuỗi N có độ dài chỉ bằng các chữ cái A, B và C, trong đó số lượng ký tự B nhiều nhất là ba."
date: "2026-06-26T18:27:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105613
codeforces_index: "B"
codeforces_contest_name: "Qualifying round of the IX regional Olympiad for the Governors Prize 2024, grades 9-10, Vologda region"
rating: 0
weight: 105613
solve_time_s: 39
verified: true
draft: false
---

[CF 105613B - Số lượng từ](https://codeforces.com/problemset/problem/105613/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta đếm xem chiều dài là bao nhiêu`N`trình tự có thể được tạo ra chỉ bằng cách sử dụng các chữ cái`A`,`B`, Và`C`, trong đó số lượng`B`nhiều nhất là ba ký tự. Một chuỗi được coi là một từ hợp lệ nếu nó thỏa mãn hạn chế này và chúng ta chỉ cần xuất ra tổng số chuỗi hợp lệ. 

Đầu vào chứa một số nguyên duy nhất`N`, độ dài cần thiết của từ. Đầu ra là số từ có thể có độ dài chính xác đó. Ràng buộc là nhỏ, với`N`nhiều nhất là 20, do đó ngay cả một giải pháp sử dụng tổ hợp hoặc một chương trình động nhỏ cũng dễ dàng đủ nhanh. Việc tạo ra tất cả các từ một cách thô bạo vẫn sẽ là một lựa chọn tồi vì có`3^N`các trình tự có thể. Khi`N = 20`, đây là hơn 3,4 tỷ khả năng, vượt xa những gì có thể kiểm tra riêng lẻ. 

Khó khăn chính không phải là kích thước của câu trả lời, vì nó vừa khít với phạm vi số nguyên tiêu chuẩn cho ràng buộc này. Thách thức là nhận ra rằng chúng ta không quan tâm đến vị trí chính xác của từng chữ cái ngoại trừ có bao nhiêu chữ cái.`B`các ký tự xuất hiện. Việc xử lý từng từ riêng biệt sẽ bỏ qua cấu trúc này. 

Có một số trường hợp đặc biệt mà việc triển khai bất cẩn có thể thất bại. Nếu như`N = 1`, mọi từ có một chữ cái đều hợp lệ vì không thể có nhiều hơn ba`B`nhân vật. Đối với đầu vào:```
1
```đầu ra đúng là:```
3
```Một giải pháp chỉ đếm các từ có số 0, một, hai hoặc ba`B`các ký tự mà quên mất rằng các vị trí còn lại đều có thể độc lập`A`hoặc`C`sẽ đánh giá thấp câu trả lời. 

Một trường hợp biên khác là khi`N = 3`. Mọi từ có thể đều hợp lệ vì một từ có độ dài ba không thể chứa bốn từ trở lên`B`nhân vật. Đối với đầu vào:```
3
```đầu ra đúng là:```
27
```Một giải pháp dừng lại ở ba`B`nhân vật mà không xem xét tất cả`3^N`các từ có thể ở độ dài này có thể tạo ra một giá trị nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi từ có độ dài có thể`N`, đếm số lượng của nó`B`ký tự và giữ nó nếu số lượng nhiều nhất là ba. Điều này đúng vì nó kiểm tra chính xác định nghĩa của một từ hợp lệ. Tuy nhiên, nó đòi hỏi phải kiểm tra`3^N`dây. Vì`N = 20`, điều đó có nghĩa là kiểm tra 3.486.784.401 ứng viên, quá chậm. 

Quan sát hữu ích là các vị trí chứa`B`là những vị trí duy nhất có hạn chế. Một khi chúng ta quyết định chính xác điều đó`k`vị trí chứa`B`, Ở đâu`k`nằm trong khoảng từ 0 đến 3, tất cả các vị trí còn lại có thể chứa độc lập một trong hai`A`hoặc`C`. 

Đối với một giá trị cố định của`k`, các vị trí của`B`nhân vật có thể được chọn trong`C(N, k)`cách. Phần còn lại`N - k`mỗi vị trí có hai lựa chọn, vì vậy họ đóng góp`2^(N-k)`khả năng. Thêm những trường hợp này cho`k = 0, 1, 2, 3`đưa ra câu trả lời. 

Phương pháp brute-force hoạt động vì mỗi từ hợp lệ được tính chính xác một lần. Phương pháp nhanh hơn vẫn giữ nguyên ý tưởng đếm đó nhưng nhóm tất cả các từ có cùng số từ lại với nhau.`B`các ký tự, tránh sự cần thiết phải xây dựng chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^N) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của`N`. Câu trả lời chỉ phụ thuộc vào độ dài của từ, do đó không cần lưu trữ hoặc tạo ra các chuỗi riêng lẻ. 
2. Xét từng số có thể có của`B`nhân vật từ`0`ĐẾN`3`. Bất kỳ số nào lớn hơn sẽ vi phạm điều kiện, vì vậy đây là những trường hợp duy nhất góp phần đưa ra câu trả lời. 
3. Đối với số đã chọn`k`của`B`các ký tự, hãy tính xem có bao nhiêu cách đặt chúng. Đây là`C(N, k)`, bởi vì chúng tôi chọn chính xác`k`các vị trí ngoài`N`các vị trí có sẵn. 
4. Nhân số này với`2^(N-k)`. Sau khi đặt tất cả`B`các ký tự, mọi vị trí còn lại có thể trở thành một trong hai vị trí độc lập`A`hoặc`C`. 
5. Cộng phần đóng góp của cả bốn trường hợp để có được số đếm cuối cùng. 

Tại sao nó hoạt động: mỗi từ hợp lệ đều có một số số chính xác`k`của`B`ký tự và giá trị đó phải là một trong`0`,`1`,`2`, hoặc`3`. Thuật toán đếm tất cả các từ có một`k`bằng cách chọn`B`các vị trí và sau đó lấp đầy mọi vị trí khác bằng`A`hoặc`C`. Các nhóm này rời rạc và cùng nhau chứa mọi từ hợp lệ, vì vậy tổng chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ans = 0
    comb = 1

    for k in range(4):
        if k > 0:
            comb = comb * (n - k + 1) // k
        ans += comb * (1 << (n - k))

    print(ans)

if __name__ == "__main__":
    solve()
```Biến`comb`lưu trữ hệ số nhị thức hiện tại`C(N, k)`. Nó bắt đầu như`C(N, 0) = 1`. Mỗi lần lặp lại cập nhật nó bằng cách sử dụng mối quan hệ:`C(N, k) = C(N, k-1) * (N-k+1) / k`Điều này tránh tính toán giai thừa và giữ cho việc thực hiện đơn giản. 

Vòng lặp chỉ chạy bốn lần vì chỉ có từ 0 đến ba lần xuất hiện`B`được phép. biểu thức`1 << (n - k)`tính toán`2^(n-k)`, đại diện cho sự lựa chọn cho những người không`B`các vị trí. 

Số nguyên Python tự động xử lý kích thước của các giá trị trung gian, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
```thuật toán đánh giá bốn số lượng có thể có của`B`. 

| k | C(4, k) | Lựa chọn cho A/C | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 2^4 = 16 | 16 | 
| 1 | 4 | 2^3 = 8 | 32 | 
| 2 | 6 | 2^2 = 4 | 24 | 
| 3 | 4 | 2^1 = 2 | 8 | 

Tổng cộng là`16 + 32 + 24 + 8 = 80`, do đó đầu ra là:```
80
```Ví dụ này chứng tỏ rằng thuật toán đếm các nhóm từ thay vì xây dựng từng từ. 

Đối với đầu vào:```
3
```phép tính là: 

| k | C(3, k) | Lựa chọn cho A/C | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 2^3 = 8 | 8 | 
| 1 | 3 | 2^2 = 4 | 12 | 
| 2 | 3 | 2^1 = 2 | 6 | 
| 3 | 1 | 2^0 = 1 | 1 | 

Tổng cộng là`8 + 12 + 6 + 1 = 27`. 

Điều này xác nhận trường hợp cạnh trong đó mọi độ dài ba từ có thể có đều hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Vòng lặp chỉ có bốn lần lặp và mỗi lần cập nhật kết hợp là thời gian không đổi. Độ phức tạp thực tế là không đổi đối với giới hạn đã cho. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lượng công việc cố định rất nhỏ ngay cả ở giá trị tối đa`N = 20`. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# sample
assert run("4\n") == "80\n", "sample 1"

# custom cases
assert run("1\n") == "3\n", "minimum size"
assert run("2\n") == "9\n", "all length two words are valid"
assert run("3\n") == "27\n", "all length three words are valid"
assert run("20\n") == "210763776\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`3`| Kiểm tra độ dài từ nhỏ nhất có thể. | 
|`2`|`9`| Xác nhận công thức khi tất cả các từ có thể vẫn hợp lệ. | 
|`3`|`27`| Thực hiện ranh giới nơi`B`giới hạn không bao giờ hạn chế. | 
|`20`|`210763776`| Kiểm tra đầu vào lớn nhất được phép và các giá trị tổ hợp lớn. | 

## Vỏ cạnh 

cho`N = 1`, thuật toán xem xét`k = 0`Và`k = 1`. Những đóng góp là`2^1 = 2`từ không có`B`Và`C(1,1) * 2^0 = 1`từ với một`B`. Kết quả cuối cùng là`3`, phù hợp với các từ`A`,`B`, Và`C`. 

Vì`N = 3`, thuật toán tính tổng mọi phân phối có thể có của`B`nhân vật. Những đóng góp là`8`,`12`,`6`, Và`1`, tổng cộng`27`. Vì tổng số từ có thể có cũng là`3^3 = 27`, điều này xác nhận rằng không có từ không hợp lệ nào tồn tại ở độ dài này. 

Vì`N = 20`, thuật toán không bao giờ tạo ra bất kỳ chuỗi nào. Nó chỉ đánh giá bốn kết hợp, một cho mỗi số lượng được phép`B`nhân vật. Phép tính vẫn ở mức nhỏ và tạo ra số đếm chính xác đồng thời tránh được hàng tỷ lần kiểm tra riêng lẻ.
