---
title: "CF 104344F - Pegadinha"
description: "Chúng ta được cấp một tòa nhà có các tầng được đánh số từ 1 đến N. Ban đầu, mỗi tầng đều tắt đèn. Một chuỗi N người đi qua và bật tắt các công tắc theo cách có cấu trúc. Người thứ i chuyển đổi mọi số tầng là bội số của i."
date: "2026-07-01T18:28:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "F"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 92
verified: false
draft: false
---

[CF 104344F - Pegadinha](https://codeforces.com/problemset/problem/104344/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tòa nhà có các tầng được đánh số từ 1 đến N. Ban đầu, mỗi tầng đều tắt đèn. Một chuỗi N người đi qua và bật tắt các công tắc theo cách có cấu trúc. Người thứ i chuyển đổi mọi số tầng là bội số của i. Sau khi tất cả N người đã làm việc này, một số tầng sẽ sáng và những tầng khác vẫn tối. 

Chuyển đổi có nghĩa là chuyển đổi trạng thái, do đó tắt trở thành bật và bật trở thành tắt. Nhiệm vụ cuối cùng không phải là mô phỏng quá trình mà là xác định chính xác tầng nào được thắp sáng ở cuối và in số tầng đó theo thứ tự tăng dần. 

Ràng buộc N có thể lớn tới 10^8, điều này ngay lập tức loại trừ mọi mô phỏng trên tất cả các tầng cho tất cả mọi người. Cách tiếp cận trực tiếp sẽ yêu cầu khoảng N thao tác cho mỗi người, dẫn đến khoảng N^2 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Ngay cả một lần chuyển qua tất cả các chuyển đổi cũng là quá lớn nếu chúng ta không cẩn thận, vì các thao tác 10^8 đã ở giới hạn 1 giây trong các ngôn ngữ được tối ưu hóa và không thể thực hiện được trong Python khi lặp lại nhiều lần. 

Khó khăn chính là nhận ra rằng quy trình này có tính cấu trúc chặt chẽ và chỉ phụ thuộc vào số ước của mỗi chỉ số sàn. 

Trường hợp cạnh tinh tế xuất hiện khi N nhỏ. Ví dụ: nếu N = 1, tầng duy nhất được bật một lần và vẫn bật. Với N = 2, tầng 1 được bật hai lần và kết thúc, trong khi tầng 2 được bật một lần và giữ nguyên. Những trường hợp nhỏ này gợi ý rằng trạng thái cuối cùng được xác định bởi tính chẵn lẻ của một cái gì đó nội tại đối với mỗi số chứ không phải là chuỗi các phép toán. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ lặp lại trên mỗi người i từ 1 đến N và sau đó lặp lại bội số của i, chuyển đổi trạng thái của các tầng đó. Điều này phản ánh trực tiếp báo cáo vấn đề và đúng về mặt logic. 

Tuy nhiên, chi phí là vấn đề. Vòng lặp bên trong chạy N/i lần, do đó tổng số lần chuyển đổi xấp xỉ N/1 + N/2 + ... + N/N, tức là N log N. Thậm chí điều đó có thể vượt qua trong C++, nhưng với N lên tới 10^8 thì điều đó hoàn toàn không khả thi trong Python và thậm chí về mặt khái niệm, chúng tôi vẫn đang lặp lại hàng tỷ thao tác trong các trường hợp xấu nhất. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì hỏi mỗi người chuyển đổi tầng bao nhiêu lần, chúng tôi hỏi mỗi tầng được chuyển đổi bao nhiêu lần. Tầng k được chuyển đổi một lần cho mỗi ước số của k, bởi vì mọi ước số i đều đóng góp một chuyển đổi khi người tôi hành động. Do đó, tầng k được chuyển đổi chính xác d(k) lần, trong đó d(k) là số ước của k. 

Một tầng sẽ sáng lên nếu nó được bật tắt một số lần lẻ. Vì vậy, bài toán quy về việc tìm tất cả k sao cho d(k) là số lẻ. Một thực tế lý thuyết số cổ điển là chỉ những hình vuông hoàn hảo mới có số ước là số lẻ. Điều này xảy ra vì các ước số thường ghép thành (a, k/a), ngoại trừ khi a = k/a, điều này chỉ xảy ra với các số chính phương. 

Vì vậy, câu trả lời chính xác là tất cả các bình phương hoàn hảo nhỏ hơn hoặc bằng N. 

Chúng ta có thể tạo chúng trực tiếp bằng cách lặp i từ 1 cho đến i^2 ≤ N và in i^2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N log N) | O(N) | Quá chậm | 
| Đếm vuông | O(√N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi tầng k được chuyển đổi một lần cho mỗi ước số của k. Điều này điều chỉnh lại toàn bộ quá trình theo cách đếm số chia thay vì mô phỏng con người. 
2. Nhận biết rằng trạng thái cuối cùng chỉ phụ thuộc vào việc số ước của k có phải là số lẻ hay không. Nếu nó là số lẻ thì cuối cùng sàn nhà sẽ sáng lên. 
3. Sử dụng thực tế lý thuyết số rằng chỉ có những hình vuông hoàn hảo mới có số ước là số lẻ. Điều này xuất phát từ việc ghép các ước số đối xứng xung quanh √k. 
4. Kết luận rằng ta chỉ cần xuất ra tất cả các số nguyên có dạng i² sao cho i² ≤ N. 
5. Lặp lại i bắt đầu từ 1 và tiếp tục trong khi i2 ≤ N, mỗi lần in i2.

Tại sao nó hoạt động: mọi số nguyên không phải là số chính phương đều có các ước số được sắp xếp theo cặp riêng biệt, tạo ra số đếm chẵn, do đó nó sẽ triệt tiêu. Các hình vuông hoàn hảo có chính xác một ước số không ghép đôi, căn bậc hai, làm cho số đếm là số lẻ và để lại trạng thái cuối cùng. Bất biến này có giá trị độc lập đối với mỗi tầng, vì vậy các bình phương tính toán sẽ xác định đầy đủ kết quả đầu ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    i = 1
    while i * i <= n:
        print(i * i)
        i += 1

if __name__ == "__main__":
    solve()
```Giải pháp đọc N và sau đó lặp lại các căn bậc hai có thể. Mỗi giá trị i đại diện cho một tầng i² tồn tại trong quá trình chuyển đổi. Điều kiện vòng lặp i * i <= n đảm bảo chúng ta không vượt quá phạm vi sàn hợp lệ. Không cần cấu trúc dữ liệu bổ sung vì kết quả đầu ra được tạo ra một cách nhanh chóng. 

Chi tiết quan trọng là sử dụng i * i thay vì căn bậc hai dấu phẩy động, tránh các vấn đề về độ chính xác và giữ cho giải pháp an toàn cho số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1: N = 3 

Chúng tôi kiểm tra các giá trị của i: 

| tôi | i² | i² 3 | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 
| 2 | 4 | không | dừng lại | 

Chỉ có tầng 1 vẫn còn sáng. 

Điều này phù hợp với thực tế là chỉ có 1 là hình vuông hoàn hảo trong phạm vi. 

### Ví dụ 2: N = 6 

| tôi | i² | i² 6 | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | 1 | 
| 2 | 4 | vâng | 4 | 
| 3 | 9 | không | dừng lại | 

Các tầng được chiếu sáng cuối cùng là 1 và 4, tương ứng chính xác với các ô vuông hoàn hảo dưới 6. 

Điều này xác nhận rằng thuật toán liệt kê trực tiếp các trạng thái còn tồn tại mà không cần mô phỏng các lần chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√N) | Chúng tôi lặp i từ 1 đến ⌊√N⌋ và in mỗi ô vuông một lần | 
| Không gian | O(1) | Không có bộ nhớ phụ ngoài một vài số nguyên | 

Ràng buộc N ≤ 10^8 ngụ ý √N ≤ 10^4, do đó vòng lặp chạy tối đa mười nghìn lần lặp, điều này không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input().strip())
    i = 1
    while i * i <= n:
        print(i * i)
        i += 1

# provided samples
assert run("3\n") == "1"
assert run("6\n") == "1\n4"
assert run("1\n") == "1"

# custom cases
assert run("2\n") == "1", "small non-square"
assert run("10\n") == "1\n4\n9", "multiple squares"
assert run("100\n") == "\n".join(str(i*i) for i in range(1, 11)), "larger square boundary"
assert run("0\n") == "", "edge zero case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | hành vi không vuông nhỏ nhất | 
| 10 | 1 4 9 | độ chính xác của nhiều đầu ra | 
| 100 | hình vuông đến ranh giới | độ chính xác của phép lặp giới hạn trên | 
| 0 | trống | trường hợp biên suy biến | 

## Vỏ cạnh 

Với N = 1 thì chỉ tồn tại tầng 1. Nó được bật một lần nên nó vẫn bật. Thuật toán đưa ra i = 1, tạo ra 1² = 1, điều này đúng. 

Đối với một số không phải là hình vuông như N = 2 thì chỉ có i = 1 thỏa i2 ≤ 2 nên chỉ in ra tầng 1. Tầng 2 không được in vì tầng 2 không phải là hình vuông hoàn hảo và có số ước chẵn, nghĩa là nó kết thúc. 

Đối với N lớn chẳng hạn như 10^8, tôi chạy tới 10^4 và chúng tôi xuất chính xác tất cả các ô vuông có kích thước lên tới 10000² = 10^8, đảm bảo không có vấn đề về tràn hoặc hiệu suất. 

Mỗi trường hợp xác nhận rằng thuật toán chỉ dựa vào phép lặp số nguyên và tránh hoàn toàn mô phỏng, duy trì tính chính xác và hiệu quả.
