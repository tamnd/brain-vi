---
title: "CF 102330C - \u041c\u044f\u0447\u0438\u043a\u0438"
description: "Petya ném một quả bóng mỗi phút về phía Vova. Khoảng cách giữa chúng là L, và mọi quả bóng đều chuyển động với vận tốc X nên một quả bóng cần L/X phút để đến được Vova. Vova không bắn bóng ngay lập tức. Anh ta bắt đầu sút khi một quả bóng ở trong khoảng cách D tính từ anh ta."
date: "2026-08-14T01:04:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "C"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 273
verified: true
draft: false
---

[CF 102330C - \u041c\u044f\u0447\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102330/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Petya ném một quả bóng mỗi phút về phía Vova. Khoảng cách giữa chúng là`L`, và mỗi quả bóng chuyển động với tốc độ`X`, vậy một quả bóng cần`L / X`phút để đến Vova. 

Vova không bắn bóng ngay lập tức. Anh ta bắt đầu sút khi bóng ở trong khoảng cách`D`của anh ấy. Súng của anh ta bắn ngay lập tức, nhưng sau mỗi lần bắn anh ta cần chính xác`M`phút trước khi nó có thể bắn trở lại. Nếu có nhiều quả bóng ở trong tầm bắn, anh ta luôn bắn quả gần nhất, đó là quả bóng già nhất trong số những quả còn sống vì mọi quả bóng đều có tốc độ như nhau. 

Chúng ta cần số quả bóng mà Petya đã ném khi quả bóng đầu tiên chạm tới Vova. Nếu Vova có thể bắn mọi quả bóng mãi mãi thì câu trả lời là`-1`. 

Các tham số đều có thể lớn bằng`10^9`. Điều đó ngay lập tức loại trừ mọi mô phỏng tỷ lệ thuận với số phút hoặc số bóng, bởi vì bản thân câu trả lời có thể xoay quanh`10^9`. Giải pháp cần thiết chỉ được sử dụng một số phép tính số học không đổi. Số nguyên Python cũng tạo ra các sản phẩm như`X * (M - 1)`an toàn mà không cần bất kỳ xử lý tràn đặc biệt nào. 

Trường hợp tế nhị đầu tiên là`M = 1`. Vova có thể sút mỗi phút một lần, thường xuyên như Petya tạo bóng. Ví dụ,`5 2 2 1`có câu trả lời`-1`. Cuối cùng, quá trình mô phỏng có thể chỉ dừng lại do giới hạn triển khai, nhưng về mặt toán học, mọi quả bóng đều bị loại bỏ trước khi đến được Vova. 

Trường hợp tinh tế thứ hai là khi một quả bóng đến Vova vào đúng thời điểm Vova có thể bắn nó. Ví dụ,`6 1 3 2`có câu trả lời`4`. Quả bóng thứ tư đến Vova cùng lúc với quả bóng thứ tư có thể thực hiện được. Trò chơi kết thúc vào thời điểm đó, vì vậy sự bình đẳng phải được coi là sự thất bại của Vova chứ không phải là một cú đánh thành công. 

Trường hợp tinh vi thứ ba là phép chia chính xác trong công thức cuối cùng. Ví dụ, với`L = 10`,`X = 2`,`D = 6`, Và`M = 2`, tỷ lệ liên quan chính xác là`6 / 2 = 3`, vậy câu trả lời là`4`, không`5`. Việc sử dụng dấu phẩy động và lấy mức trần gần đúng có thể gây ra nguồn lỗi không cần thiết, do đó việc triển khai sử dụng phép chia số nguyên. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp có thể xử lý từng quả bóng một. Đối với mỗi quả bóng, chúng tôi có thể xác định thời điểm nó đi vào trường bắn của Vova, theo dõi thời điểm súng có sẵn và quyết định xem quả bóng đó có được bắn hay không trước khi nó đến Vova. Điều này hiệu quả vì tất cả các quả bóng đều giống hệt nhau ngoại trừ thời gian phóng của chúng và quả bóng gần nhất luôn là quả bóng lâu đời nhất còn sót lại. 

Vấn đề với cách tiếp cận này là số lượng bóng. Coi như`X = 1`,`D = 10^9`, Và`M = 2`. Quả bóng đầu tiên chạm tới Vova chỉ có thể xuất hiện sau khoảng`10^9`quả bóng đã được ném. Một vòng lặp trên tất cả những quả bóng đó thực hiện khoảng một tỷ lần lặp, vượt xa giới hạn 0,5 giây. Ngay cả một mô phỏng sự kiện được tối ưu hóa cũng không giải quyết được vấn đề cơ bản vì đơn giản là có quá nhiều sự kiện. 

Quan sát quan trọng là chúng ta thực sự không cần biết từng quả bóng ở đâu. Cú đánh đầu tiên xảy ra khi quả bóng đầu tiên đạt đến khoảng cách bắn. Sau đó, mọi cảnh quay đều diễn ra chính xác`M`phút sau, miễn là vẫn còn bóng chờ sẵn. Vì Petya tung các quả bóng cách nhau một phút và`M >= 1`, cái`k`- phát bắn thứ nhất luôn nhắm vào`k`-quả bóng thứ. 

Hãy ném quả bóng đầu tiên vào đúng thời điểm`1`. Nó đạt đến ranh giới chụp sau`(L - D) / X`phút. Do đó`k`-lần bắn thứ xảy ra vào lúc`1 + (L - D) / X + (k - 1)M`. 

các`k`-quả bóng thứ tới Vova tại`k + L / X`. 

Quả bóng đầu tiên chạm tới Vova trước khi nó có thể được bắn chính xác là quả bóng đầu tiên`k`mà thời gian đến của nó nhỏ hơn hoặc bằng thời gian bắn đã lên lịch. So sánh các biểu thức này gây ra`L`hủy bỏ hoàn toàn, để lại một điều kiện chỉ liên quan đến`D`,`X`, Và`M`. 

Vì`M = 1`, bất đẳng thức thu được không bao giờ có thể trở thành đúng. Vì`M > 1`, quả bóng bị mất đầu tiên là`1 + ceil(D / (X(M - 1)))`. 

Do đó, toàn bộ mô phỏng đã bị thu gọn thành một phép chia số nguyên và một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(answer)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`L`,`X`,`D`, Và`M`. Khoảng cách`L`cuối cùng sẽ loại bỏ bất đẳng thức, nhưng nó vẫn là một phần của đầu vào. 
2. Nếu`M = 1`, in`-1`. Petya phóng một quả bóng mỗi phút và Vova cũng có thể bắn một lần mỗi phút, để súng không bao giờ bị tụt lại phía sau. 
3. Đối với`M > 1`, tính số phút mà lợi thế sút bóng thay đổi trên mỗi quả bóng. Vova tăng`M - 1`số phút trễ bổ sung so với khoảng thời gian phóng một phút của Petya. 
4. Nhân lợi thế đó với tốc độ bóng. Đại lượng tương ứng là`X * (M - 1)`. Quả bóng thua đầu tiên được xác định bằng số lượng đơn vị như vậy phù hợp với khoảng cách bắn`D`. 
5. Tính toán`ceil(D / (X * (M - 1)))`sử dụng số học số nguyên. Nếu như`q = D // denominator`Và`r = D % denominator`, trần nhà là`q`khi`r = 0`, nếu không thì`q + 1`. 
6. Thêm`1`đến trần đó và in kết quả. phần bổ sung`1`xuất phát từ thực tế là quả bóng đầu tiên có thời gian bù một phút ban đầu trước khi chuỗi cú đánh bắt đầu. 

Tại sao điều này có hiệu quả có thể được nhìn thấy trực tiếp từ thời điểm của`k`-quả bóng thứ. Thời gian đến của nó là`k + L/X`, trong khi thời gian chụp có thể là`1 + (L-D)/X + (k-1)M`. Trò chơi kết thúc khi đến điểm không muộn hơn giờ bắn, vì bình đẳng nghĩa là bóng đã tới Vova. Sắp xếp lại mang lại`k + L/X <= 1 + (L-D)/X + (k-1)M`. 

Sau khi hủy`L/X`và sắp xếp lại,`(M - 1)k >= (M - 1) + D/X`. 

nhân với`X`cho`X(M - 1)(k - 1) >= D`. 

Vì vậy nhỏ nhất có thể`k`chính xác là`1 + ceil(D / (X(M - 1)))`. 

Bất biến đằng sau đạo hàm là`k`-cú đánh thứ luôn liên quan đến`k`-quả bóng thứ. Cú đánh đầu tiên sẽ loại bỏ một quả bóng, và mỗi lần đánh sau sẽ loại bỏ quả bóng cũ nhất đang chờ trong khu vực bắn. Vì các quả bóng được phóng theo thứ tự và di chuyển với tốc độ bằng nhau nên không có quả bóng nào sau đó có thể đến gần hơn quả bóng sống sót trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L, X, D, M = map(int, input().split())

    if M == 1:
        print(-1)
        return

    denominator = X * (M - 1)
    shots_needed = (D + denominator - 1) // denominator

    print(shots_needed + 1)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý trường hợp duy nhất có câu trả lời là vô hạn. Khi`M > 1`, mẫu số`X * (M - 1)`là dương, do đó mức trần được xác định rõ ràng. 

biểu thức`(D + denominator - 1) // denominator`tính toán trần số nguyên không có dấu phẩy động. Điều này quan trọng vì tất cả các giá trị đầu vào đều là số nguyên và ranh giới nơi tỷ lệ chính xác sẽ thay đổi câu trả lời theo một. 

Biến`L`được đọc nhưng không xuất hiện trong phép tính cuối cùng. Đó là cố ý. Thời gian cần thiết để đi hết quãng đường và thời gian cần thiết để đạt đến ranh giới bắn súng đều có giá trị như nhau`L/X`thành phần, do đó nó biến mất khi hai lần được so sánh. 

Không có trạng thái mảng, hàng đợi hoặc trạng thái mỗi quả bóng trong quá trình triển khai. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý sản phẩm`X * (M - 1)`trực tiếp, có giá trị có thể gần với`10^18`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`L = 6`,`X = 1`,`D = 3`, Và`M = 2`. Quả bóng đầu tiên vào trường bắn sau`3`phút. Mỗi cú đánh tiếp theo sẽ muộn hơn hai phút, trong khi mỗi quả bóng sau được tung ra sau một phút. 

| Quả bóng`k`| Đến Vova | Có thể bắn | Kết quả | 
| --- | --- | --- | --- | 
| 1 |`7`|`4`| Bắn | 
| 2 |`8`|`6`| Bắn | 
| 3 |`9`|`8`| Bắn | 
| 4 |`10`|`10`| Đạt Vova | 

Quả bóng thứ tư là quả bóng đầu tiên có thời gian đến bằng thời gian bắn của nó. Sự bình đẳng đang thua đối với Vova nên câu trả lời là`4`, phù hợp với mẫu chính thức. 

Công thức cho`1 + ceil(3 / (1 * (2 - 1))) = 1 + 3 = 4`. 

Ví dụ này kiểm tra cụ thể ranh giới đẳng thức. Việc coi thời gian đến và thời gian chụp bằng nhau là một lần bắn thành công sẽ tạo ra câu trả lời lớn hơn một cách không chính xác. 

Đối với mẫu thứ hai,`L = 5`,`X = 2`,`D = 2`, Và`M = 1`. Petya tung ra một quả bóng mỗi phút và Vova có thể bắn một quả bóng mỗi phút. 

| Quả bóng`k`| Khoảng thời gian khởi động | Công suất súng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 phút | 1 phát mỗi phút | Bắn | 
| 2 | 1 phút | 1 phát mỗi phút | Bắn | 
| 3 | 1 phút | 1 phát mỗi phút | Bắn | 
|`k`| 1 phút | 1 phát mỗi phút | Bắn | 

Súng không bao giờ tích lũy tồn đọng nên không có quả bóng nào đến được Vova. Câu trả lời là`-1`, như trong mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Chỉ có một số lượng phép tính số học không đổi được thực hiện. | 
| Không gian |`O(1)`| Giải pháp chỉ lưu trữ bốn giá trị đầu vào và một vài số nguyên. | 

Các giá trị đầu vào lớn nhất yêu cầu số học liên quan đến các sản phẩm xung quanh`10^18`, mà Python xử lý nguyên bản. Vì thuật toán thực hiện không có vòng lặp tỷ lệ thuận với`L`,`X`,`D`, hoặc câu trả lời, nó thoải mái phù hợp với giới hạn thời gian 0,5 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# Helper: run the core solution on an input string.
def solve(data: str) -> str:
    L, X, D, M = map(int, data.split())

    if M == 1:
        return "-1"

    denominator = X * (M - 1)
    shots_needed = (D + denominator - 1) // denominator

    return str(shots_needed + 1)

def run(inp: str) -> str:
    return solve(inp).strip()

# Official samples
assert run("6 1 3 2") == "4", "sample 1"
assert run("5 2 2 1") == "-1", "sample 2"

# Minimum-size input
assert run("1 1 1 1") == "-1", "minimum values with M=1"

# All values equal
assert run("7 7 7 7") == "2", "all values equal"

# Exact division
assert run("10 2 6 2") == "4", "exact ceiling boundary"

# Non-exact division
assert run("10 3 5 3") == "2", "non-exact ceiling boundary"

# Maximum-size input
assert run("1000000000 1000000000 1000000000 1000000000") == "2", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`-1`| Giá trị tối thiểu và trường hợp sống sót vô hạn | 
|`7 7 7 7`|`2`| Các thông số bằng nhau và`M > 1`| 
|`10 2 6 2`|`4`| Phép chia số nguyên chính xác ở trần | 
|`10 3 5 3`|`2`| Trần không chính xác và xử lý từng cái một | 
|`1000000000 1000000000 1000000000 1000000000`|`2`| Ràng buộc tối đa và số học số nguyên lớn | 

## Vỏ cạnh 

Khi nào`M = 1`, hãy xem xét đầu vào chính xác`5 2 2 1`. Mẫu số của công thức chính sẽ chứa`M - 1 = 0`, do đó việc áp dụng công thức một cách mù quáng sẽ cố gắng chia cho 0. Quan trọng hơn, quá trình vật lý lại khác: Vova có thể sút một lần trong mỗi phút trong khi Petya chỉ tạo ra một quả bóng mới mỗi phút. Hàng đợi không bao giờ tăng lên nên không có quả bóng nào đến được Vova và kết quả đúng là`-1`. 

Đối với ranh giới đến đồng thời, hãy sử dụng`6 1 3 2`. Bốn lần bắn đầu tiên có thể là`4`,`6`,`8`, Và`10`. Quả bóng thứ tư đến Vova đúng lúc`10`. Bởi vì đạt Vova kết thúc trò chơi, sự bình đẳng ở thời điểm`10`được tính là một lần chạm bóng thành công chứ không phải một cú đánh thành công. Thuật toán kiểm tra`arrival <= shot`, dẫn đến`k = 4`. 

Để có ranh giới trần chính xác, hãy sử dụng`10 2 6 2`. Đây`X(M-1) = 2`, Và`D = 6`, Vì thế`D / (X(M-1)) = 3`chính xác. Câu trả lời là`1 + 3 = 4`. Một công thức sử dụng bất đẳng thức nghiêm ngặt hoặc phép chia được làm tròn không chính xác có thể trả về`3`hoặc`5`. 

Đối với ranh giới không chính xác, hãy sử dụng`10 3 5 3`. Mẫu số là`3 * 2 = 6`, trong khi`D = 5`. Tỷ lệ này nằm giữa 0 và 1, nên mức trần của nó là`1`, đưa ra câu trả lời`2`. Điều này kiểm tra xem mức trần số nguyên có được thực hiện như`(D + denominator - 1) // denominator`thay vì chia tầng thông thường.
