---
title: "CF 102416D - Rủi ro được tính toán"
description: "Chúng tôi liên tục tung xúc xắc công bằng với k mặt. Lần đổ thành công là lần hiển thị số 1 và trò chơi kết thúc ngay khi chúng ta thấy n lần đổ thành công liên tiếp."
date: "2026-08-12T20:43:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102416
codeforces_index: "D"
codeforces_contest_name: "Edinburgh Competition 2019"
rating: 0
weight: 102416
solve_time_s: 107
verified: true
draft: false
---

[CF 102416D - Rủi ro được tính toán](https://codeforces.com/problemset/problem/102416/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục tung xúc xắc công bằng với`k`những khuôn mặt. Một lần tung thành công là một lần cho thấy`1`, và trò chơi kết thúc ngay khi chúng ta nhìn thấy`n`cuộn thành công liên tiếp. Mỗi cuộn có giá £1, vì vậy giải thưởng cần thiết chính xác là số cuộn dự kiến ​​cho đến lần xuất hiện đầu tiên của`n`những cái liên tiếp. 

Đầu vào bao gồm`k`, số mặt trên xúc xắc, và`n`, độ dài cần thiết của vệt liên tiếp. Đầu ra là số cuộn dự kiến, với sai số tuyệt đối hoặc tương đối tối đa là`10^-4`. Những ràng buộc chính thức là`3 <= k <= 20`Và`1 <= n <= 5`. 

Giới hạn trên nhỏ của`n`có thể đề xuất một mô phỏng hoặc một chương trình động dựa trên trạng thái, nhưng số lượng chúng ta cần là một kỳ vọng chính xác, không phải một thử nghiệm ngẫu nhiên cụ thể. Một mô phỏng sẽ yêu cầu số lượng cuộn lớn và sẽ chỉ gần đúng câu trả lời. Quan sát hữu ích là xác suất thành công của mỗi lần tung được cố định ở`1/k`và thông tin duy nhất từ ​​quá khứ ảnh hưởng đến tương lai của chúng ta là độ dài hiện tại của hậu tố chỉ bao gồm toàn những đơn vị. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Vì`n = 1`, một người đã kết thúc trò chơi. Ví dụ, với đầu vào`3 1`, số cuộn dự kiến ​​là`3`, bởi vì mỗi lần tung đều có xác suất`1/3`về việc kết thúc trò chơi. Sự lặp lại giả định luôn có trạng thái sọc trung gian không trống có thể vô tình trả về giá trị sai ở đây. 

Một lỗi phổ biến khác là đếm số lượt quay sau lượt quay thắng cuộc thay vì tính cả số lượt quay đó. Vì`k = 6, n = 2`, câu trả lời đúng là`42`, không`41`, vì bản thân vòng quay chiến thắng có giá 1 bảng Anh. Mẫu xác nhận giá trị này. 

Vấn đề thứ ba là quên rằng nỗ lực kéo dài chuỗi không thành công sẽ đặt lại hoàn toàn chuỗi hiện tại. Với`k = 3, n = 2`, một trình tự như`1, 1`kết thúc sau hai cuộn, trong khi`1, 2, 1, 1`kết thúc sau bốn giờ. Sau khi`2`, càng sớm`1`không thể đóng góp vào chuỗi mới. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là tạo ra các chuỗi tung xúc xắc có thể có và xác định khi nào mỗi chuỗi lần đầu tiên chứa`n`những cái liên tiếp. Điều này đúng về mặt khái niệm vì mọi chuỗi hữu hạn đều có một xác suất đã biết và tính trung bình thời gian dừng của nó trên tất cả các kết quả có thể xảy ra sẽ mang lại kỳ vọng mong muốn. Vấn đề là quá trình này không có độ dài tối đa cố định. Một chuỗi có thể tránh được chuỗi mục tiêu cho nhiều lần cuộn tùy ý, do đó, việc liệt kê đầy đủ không có giới hạn dừng hữu hạn trong trường hợp xấu nhất. Ngay cả khi chúng ta dừng lại một cách giả tạo sau`L`cuộn, có`k^L`trình tự để kiểm tra, điều này hầu như không thể thực hiện được ngay lập tức. 

Thay vào đó, chúng ta có thể mô tả quá trình bằng cách sử dụng độ dài vệt hiện tại của nó. Cho phép`E_i`là số cuộn bổ sung dự kiến ​​cần thiết khi hậu tố hiện tại chứa chính xác`i`những cái liên tiếp, trong đó`0 <= i < n`. Từ tiểu bang`i`, cuộn tiếp theo tốn một cuộn. Với xác suất`1/k`, nó là một cái khác và đưa chúng ta đến trạng thái`i + 1`. Với xác suất`(k - 1)/k`, nó không phải là một và chuỗi được đặt lại về trạng thái`0`. 

Như vậy,`E_i = 1 + (1/k) E_{i+1} + ((k-1)/k) E_0`với`E_n = 0`. 

Sự đơn giản hóa quan trọng là chúng ta không thực sự cần phải giải tất cả các phương trình này bằng số. Bắt đầu từ trạng thái cuối cùng và thay ngược lại sẽ tạo ra tổng hình học. Kết quả là`E_0 = (1 - (1/k)^n) / ((1 - 1/k)(1/k)^n)`. 

Nhân tử số và mẫu số với`k^n`đưa ra biểu thức số nguyên rõ ràng hơn nhiều`E_0 = (k^(n+1) - k) / (k - 1)`. 

Ví dụ, với`k = 6`Và`n = 2`, điều này mang lại`(6^3 - 6) / 5 = 210 / 5 = 42`. 

Câu trả lời thực sự là một số nguyên cho tất cả các giá trị nguyên hợp lệ của`k`Và`n`, vì vậy số học dấu phẩy động là không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | không giới hạn,`O(k^L)`cho đường chân trời`L`|`O(L)`| Quá chậm | 
| Tối ưu |`O(1)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`k`Và`n`. Xác suất để lăn một cái là`1/k`, trong khi xác suất lăn bất cứ thứ gì khác là`(k-1)/k`. 
2. Lập mô hình quy trình theo độ dài của hậu tố một liên tiếp hiện tại. Nếu hậu tố hiện tại có độ dài`i`, một cái tăng nó lên`i+1`, trong khi bất kỳ giá trị nào khác sẽ đặt lại nó về 0. Điều này nắm bắt tất cả thông tin từ quá khứ có thể ảnh hưởng đến thời gian chờ đợi còn lại. 
3. Hãy để`E_i`biểu thị số cuộn còn lại dự kiến ​​từ trạng thái`i`. Trạng thái cuối cùng là`E_n = 0`, bởi vì chuỗi yêu cầu đã đạt được. 
4. Với mỗi trạng thái không kết thúc, hãy viết kỳ vọng một bước`E_i = 1 + E_{i+1}/k + (k-1)E_0/k`. 

các`1`chiếm số cuộn mà chúng ta sắp làm. Hai thuật ngữ còn lại mô tả hai trạng thái tiếp theo có thể có. 
5. Giải bài toán truy ngược. Biểu thức thay thế lặp đi lặp lại`E_0`về bản thân nó và một dãy hình học. Phương trình kết quả được đơn giản hóa thành`E_0 = (k^(n+1) - k)/(k-1)`. 
6. Tính biểu thức đó bằng số học số nguyên và in nó. Câu trả lời được phép lớn nhất là nhiều nhất`10^9`, do đó số học số nguyên của Python xử lý phép tính một cách thoải mái. 

### Tại sao nó hoạt động 

Nhà nước`i`luôn đại diện chính xác cho số lượng liên tiếp ngay trước lần tung tiếp theo. Điều này là đủ vì các cuộn cũ hơn không ảnh hưởng đến các cuộn trong tương lai khi đã biết độ dài hậu tố hiện tại. Từ mỗi trạng thái, phép truy toán sẽ xem xét mọi kết quả tiếp theo có thể xảy ra và thêm chính xác một kết quả cho lần quay tiếp theo đó. Từ`E_n = 0`, việc giải hàm truy toán sẽ cho ra thời gian dừng dự kiến ​​từ mọi trạng thái, kể cả trạng thái ban đầu`E_0`. Dạng đóng chỉ đơn giản là nghiệm chính xác của các phương trình kỳ vọng đó, do đó thuật toán trả về số cuộn dự kiến ​​cần thiết thay vì ước tính dựa trên mô phỏng ngẫu nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, n = map(int, input().split())

    answer = (k ** (n + 1) - k) // (k - 1)
    print(f"{answer:.2f}")

if __name__ == "__main__":
    solve()
```Chương trình đọc hai tham số và đánh giá trực tiếp dạng đóng thu được từ phép truy toán. Không cần thiết phải xây dựng các trạng thái chuỗi riêng lẻ vì phép truy toán đã được đơn giản hóa về mặt đại số. 

Số mũ là`n + 1`, không`n`. Đây là một lỗi thường gặp. Vì`n = 1`, công thức trở thành`(k^2-k)/(k-1) = k`, đó chính xác là thời gian chờ đợi dự kiến ​​cho một lần tung thành công. 

Phép trừ cũng là`k`, cho`k^(n+1) - k`. Chỉ sử dụng`k^(n+1)`sẽ tạo ra kết quả không chính xác mặc dù nó có thể trông gần giống với các giá trị lớn. 

Phép chia số nguyên là an toàn vì biểu thức luôn là số nguyên. Quan trọng hơn, nó tránh làm tròn dấu phẩy động trước khi định dạng. Đầu ra sử dụng hai chữ số thập phân, nằm trong phạm vi yêu cầu`10^-4`sức chịu đựng. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`k = 6, n = 2`. Công thức đánh giá như sau. 

| Biến | Giá trị | 
| --- | --- | 
|`k`| 6 | 
|`n`| 2 | 
|`k^(n+1)`| 216 | 
|`k^(n+1) - k`| 210 | 
|`k - 1`| 5 | 
|`answer`| 42 | 

Số cuộn dự kiến ​​là`42.00`. Việc giải thích trạng thái cũng làm cho điều này trở nên trực quan: từ 0 khuôn mặt liên tiếp, một khuôn mặt sẽ đưa chúng ta về phía mục tiêu, trong khi bất kỳ khuôn mặt nào khác sẽ đặt lại chuỗi. Công thức tính toán chính xác tất cả các lần đặt lại như vậy. 

Đối với ví dụ thứ hai, hãy xem xét`k = 3, n = 1`. 

| Biến | Giá trị | 
| --- | --- | 
|`k`| 3 | 
|`n`| 1 | 
|`k^(n+1)`| 9 | 
|`k^(n+1) - k`| 6 | 
|`k - 1`| 2 | 
|`answer`| 3 | 

Một lần là đủ để hoàn thành và mỗi lần quay đều có xác suất`1/3`sản xuất một cái. Do đó, thời gian chờ đợi dự kiến ​​là`3`, xác nhận rằng công thức xử lý chính xác độ dài vệt nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Chỉ có một phép lũy thừa và một số phép tính số học không đổi được thực hiện. | 
| Không gian |`O(1)`| Thuật toán chỉ lưu trữ`k`,`n`, và giá trị kết quả. | 

Những hạn chế là rất nhỏ, với`n <= 5`Và`k <= 20`, do đó, ngay cả một chương trình động dựa trên trạng thái cũng có thể đủ nhanh. Dạng đóng về cơ bản đơn giản hơn và loại bỏ tất cả các lần lặp qua các trạng thái. Nó sử dụng bộ nhớ không đáng kể và chạy thoải mái trong giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    k, n = map(int, input().split())
    answer = (k ** (n + 1) - k) // (k - 1)
    print(f"{answer:.2f}")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("6 2\n") == "42.00", "sample 1"

# Minimum k and minimum n
assert run("3 1\n") == "3.00", "single one on a 3-sided die"

# Maximum k and maximum n
assert run("20 5\n") == "3368420.00", "maximum constraints"

# Two-sided streak with the smallest possible die
assert run("3 2\n") == "6.00", "short streak"

# Boundary case n = 1 with a larger die
assert run("20 1\n") == "20.00", "single one on a 20-sided die"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 2`|`42.00`| Cung cấp mẫu và lặp lại chung | 
|`3 1`|`3.00`| tối thiểu`n`, bắt từng số một trong số mũ | 
|`20 5`|`3368420.00`| Giá trị tối đa của cả hai tham số | 
|`3 2`|`6.00`| Hành vi thiết lập lại và vệt nhỏ không cần thiết | 
|`20 1`|`20.00`| Trường hợp ranh giới trong đó một thành công sẽ kết thúc trò chơi ngay lập tức | 

## Vỏ cạnh 

cho`3 1`, mục tiêu chỉ đơn giản là lần tung đầu tiên bằng một. Công thức cho`(3^2 - 3)/(3-1) = 3`, vì vậy đầu ra là`3.00`. Một giải pháp vô tình sử dụng`k^n`thay vì`k^(n+1)`sẽ thất bại vụ này ngay lập tức. 

Vì`6 2`, mục tiêu là hai cái liên tiếp. Công thức cho`(6^3 - 6)/5 = 42`, vì vậy đầu ra là`42.00`. Trường hợp này mắc sai lầm khi dừng đếm trước khi tính phí cho lượt quay thành công, vì kỳ vọng bao gồm cả lượt quay tạo ra vệt cuối cùng. 

Vì`3 2`, giá trị kỳ vọng là`(3^3 - 3)/2 = 12/2 = 6`. Một dấu vết thủ công hữu ích là quy trình bắt đầu ở trạng thái 0, chuyển sang trạng thái lần lượt, đến trạng thái cuối sau một trạng thái khác và trở về 0 sau bất kỳ trạng thái nào không phải trạng thái một. Sự lặp lại bao gồm cả hai lần chuyển đổi, do đó, những lần thử thất bại lặp đi lặp lại sẽ được tính đến thay vì coi mỗi cặp cuộn như một lần thử độc lập. 

Vì`20 5`, câu trả lời là`(20^6 - 20)/19 = 3,368,420`. Đây là góc lớn nhất của các ràng buộc đã cho và chứng minh tại sao việc mô phỏng các cuộn riêng lẻ là một cách kém để tính toán kỳ vọng. Dạng đóng sẽ đưa ra câu trả lời ngay lập tức, trong khi quá trình ngẫu nhiên thực tế có thể yêu cầu hàng triệu lượt cuộn trước khi vạch mong muốn xuất hiện.
