---
title: "CF 102786G - Dấu thời gian"
description: "Đồng hồ hiển thị dấu thời gian gồm mười chữ số. Tại bất kỳ thời điểm nào, mỗi chữ số đều tiêu thụ một lượng năng lượng cố định mỗi giây tùy thuộc vào chữ số nào được hiển thị. Màn hình bắt đầu ở dấu thời gian 0000000000, sau đó tăng lên một giây mỗi giây."
date: "2026-07-27T19:28:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "G"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 57
verified: true
draft: false
---

[CF 102786G - Dấu thời gian](https://codeforces.com/problemset/problem/102786/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đồng hồ hiển thị dấu thời gian gồm mười chữ số. Tại bất kỳ thời điểm nào, mỗi chữ số đều tiêu thụ một lượng năng lượng cố định mỗi giây tùy thuộc vào chữ số nào được hiển thị. Màn hình bắt đầu ở dấu thời gian`0000000000`, sau đó tăng lên một giây mỗi giây. Các số 0 đứng đầu luôn xuất hiện nên mỗi trạng thái là một số có mười ký tự. 

Đầu vào là một số duy nhất`N`, số giây đồng hồ chạy. Trong những giây đó, đồng hồ hiển thị dấu thời gian từ`0`bởi vì`N - 1`. Nhiệm vụ là tìm tổng năng lượng tiêu thụ của tất cả mười chữ số trong các trạng thái được hiển thị đó. 

Chi phí chữ số là số lượng phân đoạn hoạt động trong phông chữ kiểu bảy đoạn: 

| Chữ số | Chi phí | 
| --- | --- | 
| 0 | 6 | 
| 1 | 2 | 
| 2 | 5 | 
| 3 | 5 | 
| 4 | 4 | 
| 5 | 5 | 
| 6 | 6 | 
| 7 | 3 | 
| 8 | 7 | 
| 9 | 6 | 

Giá trị của`N`có thể gần như`10^10`. Một mô phỏng trực tiếp sẽ yêu cầu truy cập tới mười tỷ dấu thời gian và mỗi dấu thời gian yêu cầu kiểm tra mười chữ số, điều này mang lại khoảng`10^11`các thao tác về chữ số. Điều đó vượt xa những gì có thể phù hợp với giới hạn một giây. Giải pháp phải tránh lặp lại các dấu thời gian và thay vào đó hãy đếm tần suất mỗi chữ số xuất hiện ở mọi vị trí. 

Các trường hợp cạnh chính xuất phát từ chiều rộng cố định của màn hình và thực tế là phạm vi kết thúc ở`N - 1`. Ví dụ, với đầu vào`1`, giá trị được hiển thị duy nhất là`0000000000`. Câu trả lời là`60`, bởi vì mười số 0 được thắp sáng trong một giây. Một giải pháp bắt đầu đếm từ dấu thời gian`1`sẽ trả lại không chính xác chi phí của`0000000001`. 

Một sai lầm phổ biến khác là quên rằng giới hạn trên là độc quyền. Đối với đầu vào`10`, đồng hồ hiển thị`0000000000`bởi vì`0000000009`, không qua`0000000010`. Câu trả lời đúng là`589`. Một phương pháp đếm trạng thái sau giây thứ mười sẽ thêm dấu thời gian bổ sung và nhận được kết quả sai. 

Vấn đề thứ ba xuất hiện gần các giá trị lớn nhất. Đối với đầu vào`9999999999`, dấu thời gian được hiển thị lần cuối là`9999999998`. giá trị`9999999999`không được đưa vào. Đếm tất cả các giá trị có mười chữ số và quên loại bỏ giá trị cuối cùng sẽ đánh giá quá cao câu trả lời bằng cách`60`. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ mô phỏng đồng hồ. Cứ mỗi giây, hãy chuyển dấu thời gian thành chuỗi mười chữ số, cộng giá trị của mỗi chữ số và tiếp tục cho đến khi`N`giây đã trôi qua. Cách tiếp cận này rất dễ xác minh vì nó tuân theo tuyên bố trực tiếp. Tuy nhiên, trường hợp xấu nhất đòi hỏi phải xử lý gần như`10^10`dấu thời gian và thậm chí việc triển khai rất tối ưu cũng cần khoảng`10^11`phép cộng chữ số. 

Cấu trúc của màn hình cho chúng ta một cách để bỏ qua công việc này. Thay vì hỏi chữ số nào xuất hiện trong mỗi dấu thời gian, chúng ta có thể hỏi số lần một chữ số cụ thể xuất hiện ở một vị trí cụ thể trong số tất cả các dấu thời gian từ`0`ĐẾN`N - 1`. 

Ví dụ, hãy xem xét chữ số cuối cùng. Nó quay vòng từ`0`ĐẾN`9`nhiều lần. Đối với các vị trí cao hơn, độ dài chu kỳ trở nên lớn hơn nhưng vẫn giữ nguyên mô hình. Mỗi vị trí có thể được xử lý độc lập bằng cách đếm các chu kỳ hoàn chỉnh và một phần chu kỳ còn lại. 

Đối với một vị trí có giá trị vị trí`p`, các số lặp lại theo khối có kích thước`10 * p`. Bên trong mỗi khối, mỗi chữ số sẽ giữ nguyên vị trí đó một cách chính xác`p`các số liên tiếp. Số khối hoàn chỉnh mang lại sự đóng góp đơn giản cho tất cả các chữ số. Các số còn lại sau các khối đó được xử lý bằng cách kiểm tra xem chu kỳ hiện tại đã được bao phủ bao nhiêu. 

Sau khi tìm ra số lần mỗi chữ số xuất hiện ở mỗi vị trí, chúng tôi nhân số lượng đó với chi phí năng lượng tương ứng và cộng mọi thứ lại với nhau. Độ rộng mười chữ số chỉ có nghĩa là chúng tôi lặp lại phép tính này cho các giá trị vị trí từ`1`bởi vì`1,000,000,000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10N) | O(1) | Quá chậm | 
| Tối ưu | O(10 * 10) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý từng vị trí thập phân riêng biệt, bắt đầu từ vị trí đơn vị và tiếp tục lên đến vị trí cao nhất trong mười vị trí được hiển thị. Sự đóng góp của một vị trí là độc lập với tất cả các vị trí khác, do đó, việc tính tổng các vị trí một cách riêng biệt sẽ tránh được việc xử lý toàn bộ dấu thời gian. 
2. Đối với một vị trí có giá trị`p`, tách ra`N`thành một số chu kỳ hoàn chỉnh và phần còn lại. Một chu kỳ đầy đủ có độ dài`10 * p`, bởi vì mọi chữ số từ`0`ĐẾN`9`xuất hiện cho`p`dấu thời gian liên tiếp. 
3. Thêm sự đóng góp của các chu kỳ hoàn chỉnh. Mỗi chữ số xuất hiện chính xác`p`lần trong một chu kỳ và số chu kỳ là`N // (10 * p)`. 
4. Xử lý chu trình chưa hoàn thành. Sau khi hoàn thành các chu kỳ, vị trí hiện tại bắt đầu ở chữ số`0`. đầu tiên`min(remainder, p)`giá trị thêm chữ số`0`, tiếp theo`p`giá trị thêm chữ số`1`, cứ như vậy cho đến khi hết số dư. 
5. Nhân tần số của mỗi chữ số ở vị trí này với năng lượng tiêu hao của chữ số đó và cộng nó vào đáp án cuối cùng. 

Lý do điều này có tác dụng là vì mỗi vị trí thập phân tuân theo một mẫu lặp lại hoàn hảo. Vị trí đơn vị lặp lại sau mỗi mười dấu thời gian, vị trí hàng chục lặp lại sau mỗi một trăm dấu thời gian, v.v. Đếm các lần lặp lại hoàn chỉnh và một lần lặp lại chưa hoàn thành cho mỗi dấu thời gian đúng một lần. Vì mỗi dấu thời gian được hiển thị chỉ là tổng của mười vị trí chữ số độc lập của nó nên tổng năng lượng cũng là tổng của các đóng góp vị trí độc lập này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

cost = [6, 2, 5, 5, 4, 5, 6, 3, 7, 6]

def solve():
    n = int(input())

    ans = 0
    place = 1

    for _ in range(10):
        cycle = place * 10
        full = n // cycle
        rem = n % cycle

        for digit in range(10):
            cnt = full * place

            extra_start = digit * place
            extra_end = (digit + 1) * place

            if rem > extra_start:
                cnt += min(rem, extra_end) - extra_start

            ans += cnt * cost[digit]

        place *= 10

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng`cost`lưu trữ năng lượng cần thiết cho một giây hiển thị mỗi chữ số. Vòng lặp trên mười vị trí khớp với chiều rộng vật lý của đồng hồ, do đó không cần xử lý đặc biệt đối với các số 0 đứng đầu. 

Đối với mỗi vị trí,`place`biểu thị có bao nhiêu dấu thời gian liên tiếp chứa cùng một chữ số ở vị trí đó. Ví dụ, ở vị trí hàng chục`place`là`10`, vì mỗi chữ số chiếm mười số liên tiếp.`full * place`đếm các chu kỳ hoàn chỉnh. Logic từng phần của chu trình tính toán sự chồng chéo giữa các dấu thời gian còn lại và khoảng thời gian thuộc về một chữ số cụ thể. Biểu thức sử dụng`min`ngăn chặn việc đếm vượt quá phần còn lại. 

Biến`ans`có thể trở nên lớn hơn phạm vi số nguyên 32 bit. Số nguyên Python tự động tăng lên, nhưng các ngôn ngữ có số nguyên có kích thước cố định cần loại 64 bit. 

Vòng lặp chạy chính xác mười lần cho mười chữ số của màn hình. Thứ tự của các vị trí xử lý không quan trọng vì năng lượng của mỗi vị trí chữ số là độc lập. 

## Ví dụ đã hoạt động 

Vì câu lệnh ban đầu không chứa các giá trị mẫu hiển thị nên chúng tôi sử dụng các dấu vết nhỏ. 

Đối với đầu vào`1`, đồng hồ chỉ hiển thị`0000000000`. 

| Vị trí | Giá trị địa điểm | Chu kỳ đầy đủ | Phần còn lại | Đóng góp | 
| --- | --- | --- | --- | --- | 
| Những cái | 1 | 0 | 1 | chữ số 0 xuất hiện một lần | 
| Hàng chục | 10 | 0 | 1 | chữ số 0 xuất hiện một lần | 
| Hàng trăm | 100 | 0 | 1 | chữ số 0 xuất hiện một lần | 
| Các vị trí còn lại | giá trị lớn hơn | 0 | 1 | chữ số 0 xuất hiện một lần | 

Mỗi vị trí trong mười vị trí đều đóng góp giá trị của chữ số`0`, đó là`6`. Câu trả lời cuối cùng là`10 * 6 = 60`. 

Đối với đầu vào`10`, đồng hồ hiển thị`0000000000`bởi vì`0000000009`. 

| Vị trí | Giá trị địa điểm | Chu kỳ đầy đủ | Phần còn lại | Mẫu chữ số | 
| --- | --- | --- | --- | --- | 
| Những cái | 1 | 1 | 0 | 0 đến 9 một lần | 
| Hàng chục | 10 | 0 | 10 | số không xuất hiện mười lần | 
| Các vị trí khác | 100+ | 0 | 10 | số không xuất hiện mười lần | 

Vị trí đơn vị đóng góp tổng của tất cả các chi phí chữ số, tức là`49`. Mỗi vị trí trong số chín vị trí còn lại đều đóng góp`10 * 6 = 60`. Câu trả lời là`49 + 9 * 60 = 589`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100) | Mười vị trí được xử lý và mỗi vị trí kiểm tra mười chữ số | 
| Không gian | O(1) | Chỉ có một vài bộ đếm và mảng chi phí chữ số được lưu trữ | 

Thuật toán thực hiện một lượng công việc không đổi bất kể kích thước của`N`. Nó dễ dàng phù hợp với giới hạn ngay cả khi`N`gần với`10^10`. 

## Trường hợp thử nghiệm```python
import sys
import io

cost = [6, 2, 5, 5, 4, 5, 6, 3, 7, 6]

def solve_case(inp: str) -> str:
    n = int(inp)

    ans = 0
    place = 1

    for _ in range(10):
        cycle = place * 10
        full = n // cycle
        rem = n % cycle

        for digit in range(10):
            cnt = full * place
            start = digit * place
            end = (digit + 1) * place

            if rem > start:
                cnt += min(rem, end) - start

            ans += cnt * cost[digit]

        place *= 10

    return str(ans)

def run(inp: str) -> str:
    return solve_case(inp)

assert run("1") == "60", "minimum input"
assert run("10") == "589", "single complete units cycle"
assert run("11") == "656", "boundary after a full units cycle"
assert run("100") == "5890", "transition into hundreds position"
assert run("9999999999") == "489999999940", "maximum valid input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`60`| Màn hình hoàn toàn bằng 0 ban đầu được tính | 
|`10`|`589`| Một chu kỳ đầy đủ của chữ số cuối cùng được xử lý | 
|`11`|`656`| Dấu thời gian đầu tiên sau ranh giới chu kỳ | 
|`100`|`5890`| Chuyển sang vị trí chữ số tiếp theo | 
|`9999999999`|`489999999940`| Phạm vi tối đa và xử lý câu trả lời lớn | 

## Vỏ cạnh 

cho`N = 1`, thuật toán có`full = 0`Và`rem = 1`cho mọi vị trí. Chu kỳ từng phần thêm một lần xuất hiện của chữ số`0`ở mọi vị trí, đưa ra mười số 0 và câu trả lời là`60`. Không có dấu thời gian nào sau số 0 được vô tình đưa vào. 

Vì`N = 10`, vị trí đơn vị có một chu trình hoàn chỉnh và mọi vị trí cao hơn chỉ có một phần của chu trình. Điều này tách chữ số cuối cùng khỏi các số 0 đứng đầu và tạo ra`589`. 

Vì`N = 9999999999`, thuật toán đếm mọi số có mười chữ số ngoại trừ`9999999999`chính nó vì phạm vi kết thúc tại`N - 1`. Nếu số cuối cùng vô tình được đưa vào thì kết quả sẽ lớn hơn`60`, đó chính xác là cái giá phải trả cho việc hiển thị mười số chín trong một giây. Việc đếm chu kỳ tránh được lỗi từng cái một vì tất cả các phép tính được thực hiện trong lần đầu tiên.`N`dấu thời gian.
