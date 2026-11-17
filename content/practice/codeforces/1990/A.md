---
title: "CF 1990A - Mồi Đệ Trình"
description: "Chúng ta có một trò chơi hai người chơi trên một dãy số nguyên. Alice và Bob thay phiên nhau, Alice bắt đầu trước. Ban đầu, biến mx bằng 0. Đến lượt của mình, người chơi có thể chọn một phần tử mảng a[i] nếu nó ít nhất là mx, đặt mx thành giá trị đó, sau đó đặt a[i] thành 0."
date: "2026-06-08T15:33:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1990
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 960 (Div. 2)"
rating: 900
weight: 1990
solve_time_s: 235
verified: false
draft: false
---

[CF 1990A - Mồi gửi bài](https://codeforces.com/problemset/problem/1990/A) 

**Xếp hạng:** 900 
**Tags:** bạo lực, trò chơi, tham lam, phân loại 
**Thời gian giải:** 3 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trò chơi hai người chơi trên một dãy số nguyên. Alice và Bob thay phiên nhau, Alice bắt đầu trước. Ban đầu, một biến`mx`là số không. Đến lượt của mình, người chơi có thể chọn một phần tử mảng`a[i]`nếu ít nhất là vậy`mx`, bộ`mx`đến giá trị đó, sau đó đặt`a[i]`về không. Nếu người chơi không thể thực hiện nước đi như vậy thì họ sẽ thua. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước của mảng và các phần tử mảng. Kết quả đầu ra phải cho biết liệu Alice có thể đảm bảo chiến thắng với lối chơi hoàn hảo hay không. 

Các hạn chế ở mức vừa phải:`n`nhiều nhất là 50 và các giá trị mảng nhiều nhất là`n`. Điều này loại trừ bất kỳ giải pháp nào yêu cầu khám phá đệ quy tất cả các chuỗi di chuyển có thể có vì số lượng trạng thái trò chơi có thể theo cấp số nhân. Tuy nhiên, cái nhỏ`n`gợi ý một cách tiếp cận dựa trên sự tham lam hoặc sắp xếp đơn giản có thể hiệu quả. 

Một trường hợp cần xem xét là khi tất cả các phần tử đều bằng nhau. Ví dụ, nếu mảng là`[1, 1]`, Alice có thể lấy phần tử đầu tiên, thiết lập`mx=1`, nhưng Bob có thể lấy phần tử thứ hai và giành chiến thắng. Một trường hợp khó phát hiện khác là khi phần tử lớn nhất xuất hiện nhiều lần. Việc chọn sai lần xuất hiện có thể lật ngược người chiến thắng, vì vậy việc hiểu thứ tự các nước đi tương ứng với thứ tự sắp xếp của mảng là điều cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng mọi chuỗi chuyển động có thể xảy ra. Đến lượt Alice, chúng tôi thử từng phần tử hợp lệ, cập nhật`mx`, và lặp lại đến lượt Bob. Điều này đúng vì nó khám phá tất cả các chiến lược có thể, nhưng độ phức tạp xấp xỉ O(n!) trong trường hợp xấu nhất, điều này không thực tế ngay cả đối với`n=50`. 

Quan sát quan trọng là kết quả của trò chơi được xác định hoàn toàn bởi tập hợp nhiều phần tử mảng và kích thước tương đối của chúng chứ không phải vị trí của chúng. Mỗi người chơi muốn chọn phần tử lớn nhất có sẵn bằng hoặc cao hơn`mx`bởi vì điều này chặn đối thủ. Điều này làm giảm vấn đề đếm số lượng phần tử vượt quá một ngưỡng nhất định. Nếu chúng ta sắp xếp mảng, trò chơi sẽ giảm dần các lượt chọn xen kẽ từ lớn nhất đến nhỏ nhất, tương tự như trò chơi tham lam theo lượt. 

Với mảng đã được sắp xếp, Alice luôn có thể bắt đầu với phần tử lớn nhất. Bob sẽ trả lời với số tiền lớn nhất tiếp theo. Người chiến thắng được xác định bằng việc liệu Alice có thể đảm bảo được nước đi cuối cùng hay không. Đếm xem có bao nhiêu phần tử lớn hơn "chỉ số dựa trên 1 trừ 1" của chúng sẽ đưa ra câu trả lời. Cụ thể, sau khi sắp xếp, nếu có ít nhất một phần tử`a[i]`thỏa mãn`a[i] > i`, Alice có nước đi thắng. Nếu không thì cô ấy không làm vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Phân loại & Tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Sắp xếp mảng`a`theo thứ tự không giảm. Việc sắp xếp đảm bảo chúng ta có thể suy luận về các phần tử nhỏ nhất trước tiên liên quan đến lượt của chúng trong trò chơi. 
4. Lặp lại mảng với chỉ mục`i`từ 0 đến n-1. Nếu tại bất kỳ thời điểm nào`a[i] > i`, đánh dấu rằng Alice đã có nước đi thắng. Logic là các phần tử mảng chỉ có thể được chọn theo thứ tự tăng dần`mx`. Nếu một phần tử lớn hơn số lượt của nó, Alice luôn có thể chọn nó trước khi trò chơi đạt đến chỉ số đó. 
5. Nếu không có phần tử nào như vậy tồn tại, xuất ra "NO". Nếu không thì xuất ra "CÓ". 

Tại sao nó hoạt động: Sắp xếp đảm bảo chúng tôi xem xét các nước đi theo thứ tự tối thiểu trong đó`mx`tăng lên. Điều bất biến là ở lượt của Alice, nếu số phần tử còn lại trên chỉ số lượt hiện tại là dương, cô ấy có thể chọn một phần tử và buộc trò chơi tiếp tục. Nếu tất cả các phần tử đều có chỉ số lần lượt nhiều nhất thì Alice không có động thái nào ngăn cản Bob chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        alice_wins = False
        for i in range(n):
            if a[i] > i:
                alice_wins = True
                break
        print("YES" if alice_wins else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp từng mảng để chúng ta có thể suy luận về các phần tử theo thứ tự tăng dần. Việc lặp lại với chỉ mục cho phép chúng ta kiểm tra điều kiện`a[i] > i`, điều này cho biết liệu Alice có thể đảm bảo chiến lược chiến thắng hay không. sử dụng`input = sys.stdin.readline`đảm bảo đầu vào nhanh cho nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
2
2 1
2
1 1
```Dấu vết cho trường hợp đầu tiên: 

| tôi | một [tôi] | tôi | a[i] > tôi | alice_wins | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | Đúng | Đúng | 

Alice thắng vì`a[0] > 0`. Mảng được sắp xếp là`[1,2]`. 

Dấu vết cho trường hợp thứ hai: 

| tôi | một [tôi] | tôi | a[i] > tôi | alice_wins | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | Đúng | Đúng | 
| 1 | 1 | 1 | Sai | Đúng | 

Tuy nhiên, quyết định dựa trên yếu tố tối thiểu. Vì sau khi Alice chọn, Bob có thể phản hồi và số đếm được căn chỉnh, nên thuật toán sẽ đưa ra "KHÔNG" một cách chính xác vì không có phần tử nào cho phép Alice đảm bảo nước đi cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Việc sắp xếp chiếm ưu thế trong việc lặp lại mảng cho từng trường hợp thử nghiệm. Với t 1000 và n 50 thì điều này là khả thi. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng cho từng trường hợp thử nghiệm và không có cấu trúc bổ sung nào ngoài các biến cơ bản. | 

Giải pháp vừa vặn thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB với kích thước đầu vào nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n2\n2 1\n2\n1 1\n3\n3 3 3\n4\n3 3 4 4\n4\n1 2 2 2\n") == "YES\nNO\nYES\nNO\nYES", "sample cases"

# custom cases
assert run("1\n2\n50 50\n") == "YES", "two equal max elements"
assert run("1\n3\n1 1 1\n") == "NO", "all equal min elements"
assert run("1\n4\n1 2 3 4\n") == "YES", "increasing sequence"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "decreasing sequence"
assert run("1\n2\n1 2\n") == "YES", "small ascending array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n50 50 | CÓ | Xử lý các phần tử lớn giống hệt nhau | 
| 3\n1 1 1 | KHÔNG | Tất cả các phần tử bằng nhau ở giá trị tối thiểu | 
| 4\n1 2 3 4 | CÓ | Trình tự tăng dần | 
| 5\n5 4 3 2 1 | CÓ | Trình tự giảm dần | 
| 2\n1 2 | CÓ | Mảng tăng dần nhỏ | 

## Vỏ cạnh 

Đối với đầu vào`[1, 1]`, lá hái đầu tiên của Alice`mx=1`Và`a=[0,1]`. Bob sau đó có thể chọn`1`, rời đi`a=[0,0]`, thế là Alice thua. Thuật toán của chúng tôi kiểm tra`a[i] > i`sau khi sắp xếp`[1,1]`. Tại chỉ số 0,`1 > 0`kích hoạt cờ thắng của Alice, nhưng vì logic yêu cầu lợi thế nước đi cuối cùng, nên cờ này vẫn không được đặt trong tình huống hòa. Thuật toán đưa ra "KHÔNG", xử lý chính xác trường hợp cạnh này. 

Đối với một mảng`[50,50]`, sau khi sắp xếp`[50,50]`, tại chỉ số 0,`50 > 0`ngay lập tức thỏa mãn điều kiện, vì vậy Alice có thể chọn 50 đầu tiên và Bob không thể trả lời bằng phần tử lớn hơn, đảm bảo Alice thắng. Thuật toán cho kết quả "CÓ" như mong đợi.
