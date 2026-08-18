---
title: "CF 102263G - Trò Chơi Bài"
description: "Cả hai người chơi đều bắt đầu với cùng một bộ bài, chứa mọi số nguyên từ 1 đến n. Trong n lượt, mỗi lá bài được mỗi người chơi sử dụng đúng một lần vì những lá bài đã chọn sẽ bị loại bỏ."
date: "2026-08-17T20:02:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "G"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 126
verified: true
draft: false
---

[CF 102263G - Trò chơi bài](https://codeforces.com/problemset/problem/102263/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cả hai người chơi đều bắt đầu với cùng một bộ bài, chứa mọi số nguyên từ 1 đến`n`. Trong thời gian`n`lượt, mỗi lá bài được mỗi người chơi sử dụng chính xác một lần vì các lá bài đã chọn sẽ bị loại bỏ. Hai người chơi tạo ra các hoán vị ngẫu nhiên độc lập của các lá bài một cách hiệu quả và lần lượt chuyển`i`so sánh vị trí chiếm giữ các thẻ`i`trong các hoán vị đó. 

Ehab kiếm được điểm bất cứ khi nào thẻ của anh ấy lớn hơn thẻ của Zeyad. Nếu E-háp chơi`k`với quân bài nhỏ hơn, anh ta nhận được chính xác`k`điểm. Thẻ bằng nhau không có điểm. 

Đầu vào chỉ chứa`n`, với`1 <= n <= 10^6`. Giới hạn trên lớn loại trừ mọi thứ bậc hai trong`n`. Thậm chí một`O(n^2)`tính toán sẽ yêu cầu`10^12`số lần lặp ở kích thước tối đa, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường có thể hỗ trợ. Chúng ta cần quy toàn bộ trò chơi về một lượng số học không đổi. 

Trường hợp cạnh đầu tiên là`n = 1`. Cả hai người chơi chỉ có thẻ`1`, vì vậy họ phải đấu với nhau và không ai ghi bàn. Đầu ra đúng là`0`. 

Trường hợp cạnh thứ hai là một bộ bài nhỏ như`n = 2`. E-háp có thẻ`1`Và`2`. Thẻ`1`không bao giờ có thể ghi bàn, trong khi thẻ`2`ghi điểm chính xác khi nó được ghép nối với`1`. Vì quân bài của đối phương được ghép với`2`có khả năng như nhau`1`hoặc`2`, đóng góp dự kiến ​​của nó là`2 * 1/2 = 1`. Như vậy đáp án đúng là`1`. Một giải pháp bất cẩn cho rằng mọi lá bài luôn ghi điểm với thứ gì đó nhỏ hơn sẽ được tính không chính xác`1 + 2 = 3`. 

Trường hợp tế nhị thứ ba là sự bình đẳng. Khi cả hai người chơi chọn cùng một số thì không người chơi nào nhận được điểm. Vì mỗi lá bài xuất hiện đúng một lần trong mỗi bộ bài nên việc ghép đôi ngẫu nhiên có thể xảy ra sự bình đẳng. Bất kỳ công thức nào tính tất cả các đối thủ có giá trị nhiều nhất`k`thay vì hoàn toàn ít hơn`k`giới thiệu một lỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp có thể tính toán riêng mức đóng góp dự kiến của từng thẻ Ehab. Đối với một thẻ`k`, chúng tôi có thể kiểm tra tất cả`n`những lá bài có thể mà Zeyad có thể chơi để chống lại nó, xác định xem liệu`k`thắng và tính trung bình các số điểm thu được. Làm điều này cho tất cả`k`yêu cầu chính xác`n^2`kiểm tra cặp. Tại`n = 10^6`, đó là`10^12`nên mặc dù phép tính đúng về mặt toán học nhưng vẫn quá chậm. 

Chúng ta có thể làm tốt hơn nhiều bằng cách xem xét từng lá bài một cách riêng biệt. Vì bộ bài của Zeyad là hoán vị ngẫu nhiên đồng đều nên lá bài được ghép với lá bài của Ehab`k`được phân bổ đồng đều cho tất cả mọi người`n`thẻ. Có chính xác`k - 1`thẻ nhỏ hơn`k`, và điểm Ehab`k`chính xác là khi một trong những lá bài đó được ghép với anh ta. 

Vậy xác suất thẻ đó`k`điểm số chỉ đơn giản là 

n k−1 ​ . 

Do đó, sự đóng góp dự kiến của nó là 

k n k−1 ​ . 

Tính tuyến tính của kỳ vọng giờ đây đã loại bỏ nhu cầu lý luận về sự phụ thuộc giữa các lượt khác nhau. Chúng ta có thể cộng phần đóng góp dự kiến ​​của mỗi thẻ Ehab mặc dù các sự kiện liên quan đến các thẻ khác nhau không độc lập. 

Tổng kỳ vọng là 

E= k=1 ∑ n ​ k n k−1 ​ = n 1 ​ k=1 ∑ n ​ (k 2 −k). 

Sử dụng tổng tiêu chuẩn 

k=1 ∑ n ​ k 2 = 6 n(n+1)(2n+1) ​ 

và 

k=1 ∑ n ​ k= 2 n(n+1) ​ , 

chúng tôi có được 

E= n 1 ​ ( 6 n(n+1)(2n+1) ​ − 2 n(n+1) ​ ). 

Phân tích nhân tử và đơn giản hóa mang lại 

E= 3 n 2 −1 ​ . 

Do đó, toàn bộ trò chơi ngẫu nhiên có thể được thay thế bằng một công thức thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, xác định bộ bài hoàn chỉnh trong cả hai bộ bài. 
2. Xét lá bài Ehab tùy ý`k`. Lá bài của đối phương được ghép với nó được phân bổ đồng đều trên tất cả`n`các thẻ có thể có vì hai hoán vị ngẫu nhiên là độc lập. 
3. Đếm những lá bài để Ehab ghi điểm`k`. Chính xác`k - 1`thẻ có giá trị nhỏ hơn`k`, vậy xác suất ghi điểm của lá bài này là`(k - 1) / n`. 
4. Nhân xác suất đó với số điểm kiếm được khi thẻ ghi điểm. Đóng góp dự kiến ​​của`k`là`k * (k - 1) / n`. 
5. Tổng hợp những đóng góp này qua mỗi`k`từ`1`bởi vì`n`. Bằng tính tuyến tính của kỳ vọng, chúng ta có thể cộng thêm những đóng góp dự kiến ​​mà không yêu cầu các lượt riêng lẻ phải độc lập. 
6. Rút gọn tổng kết quả để có được`(n² - 1) / 3`, sau đó in nó dưới dạng số thực. 

### Tại sao nó hoạt động 

Đối với mỗi thẻ cố định`k`, chính xác`k - 1`của đối phương`n`quân bài khiến Ehab ghi bàn`k`điểm. Vì mọi lá bài có thể có của đối thủ đều có khả năng được ghép với nhau như nhau`k`, đóng góp dự kiến ​​của nó chính xác là`k(k-1)/n`. Mỗi lá bài trong bộ bài của Ehab được sử dụng đúng một lần nên tổng số điểm là tổng của những đóng góp riêng lẻ này. Tính tuyến tính của kỳ vọng đảm bảo rằng tổng này bằng tổng số điểm dự kiến ​​bất kể sự phụ thuộc nào giữa các cặp. Đại số giảm số tiền đó thành`(n² - 1)/3`, do đó thuật toán tính toán kỳ vọng chính xác. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline
n = int(input())
answer = (n * n - 1) / 3.0print(f"{answer:.10f}")
```Đầu vào chỉ chứa một số nguyên, do đó không cần cấu trúc dữ liệu bổ sung hoặc xử lý trường hợp kiểm thử lặp lại. 

biểu thức`n * n - 1`xuất phát trực tiếp từ kỳ vọng đơn giản hóa. Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn số nguyên ngay cả ở`n = 10^6`. 

Sự phân chia theo`3.0`tạo ra kết quả dấu phẩy động phù hợp với dung sai đầu ra được yêu cầu. In mười chữ số sau dấu thập phân cho độ chính xác cao hơn đáng kể so với yêu cầu`10^-6`. 

Phép trừ phải xảy ra trước phép chia. Vì`n = 1`, biểu thức trở thành`(1 * 1 - 1) / 3 = 0`, xử lý chính xác bộ bài nhỏ nhất có thể. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên tương ứng với`n = 3`. 

| Thẻ`k`| Thẻ đối thủ nhỏ hơn | Xác suất ghi điểm | Đóng góp dự kiến ​​| 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 1/3 | 2/3 | 
| 3 | 2 | 2/3 | 2 | 

Thêm những đóng góp mang lại 

0+ 3 2 ​ +2= 3 8 ​ =2.6666666667. 

Điều này chứng tỏ quân bài lớn nhất đóng góp phần lớn số điểm dự kiến, còn quân bài nhỏ nhất chẳng đóng góp gì vì nó không thể đánh bại được quân bài nào. 

Mẫu thứ hai tương ứng với`n = 7`. 

| Thẻ`k`| Thẻ đối thủ nhỏ hơn | Xác suất ghi điểm | Đóng góp dự kiến ​​| 
| --- | --- | --- | --- | 
| 1 | 0 | 0/7 | 0 | 
| 2 | 1 | 7/1 | 7/2 | 
| 3 | 2 | 7/2 | 7/6 | 
| 4 | 3 | 7/3 | 7/12 | 
| 5 | 4 | 7/4 | 20/7 | 
| 6 | 5 | 7/5 | 30/7 | 
| 7 | 6 | 7/6 | 42/7 | 

Tổng số tiền là 

7 2+6+12+20+30+42 ​ = 7 112 ​ =16. 

Công thức đơn giản hóa cho kết quả tương tự ngay lập tức: 

3 7 2 −1 ​ = 3 48 ​ =16. 

Bảng này cũng làm cho việc giải thích xác suất trở nên cụ thể hơn. Đối với thẻ`7`, bất kỳ lá bài nào trong số sáu lá bài còn lại đều khiến Ehab ghi điểm khi đấu với lá bài khác`7`không tạo ra điểm nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng phép tính số học không đổi được thực hiện. | 
| Không gian | O(1) | Chỉ một`n`và giá trị kết quả được lưu trữ. | 

Tối đa`n`là`10^6`, nhưng thuật toán hoàn toàn không lặp lại trên các thẻ. Thời gian chạy và mức sử dụng bộ nhớ của nó không đổi bất kể`n`, làm cho nó phù hợp một cách thoải mái với ràng buộc đã cho. 

## Trường hợp thử nghiệm```python
Python# helper: run solution on input string, return output stringimport sysimport io
def solve(inp: str) -> str:    data = inp.strip().split()    n = int(data[0])    answer = (n * n - 1) / 3.0    return f"{answer:.10f}\n"
def run(inp: str) -> str:    return solve(inp)
# provided samplesassert run("3\n") == "2.6666666667\n", "sample 1"assert run("7\n") == "16.0000000000\n", "sample 2"
# minimum-size inputassert run("1\n") == "0.0000000000\n", "only equal cards"
# n = 2, catches incorrect handling of equalityassert run("2\n") == "1.0000000000\n", "two-card deck"
# n = 4, checks the formula on a small nontrivial caseassert run("4\n") == "5.0000000000\n", "small general case"
# maximum-size inputassert run("1000000\n") == "333333333333.0000000000\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0.0000000000`| Bộ bài tối thiểu và bình đẳng | 
|`2`|`1.0000000000`| Điều kiện đối thủ nhỏ hơn hoàn toàn | 
|`4`|`5.0000000000`| Công thức chung trên bộ bài nhỏ | 
|`1000000`|`333333333333.0000000000`| Phạm vi ranh giới và số học tối đa | 

## Vỏ cạnh 

cho`n = 1`, đầu vào chỉ đơn giản là`1`. Sự ghép đôi duy nhất có thể là thẻ`1`chống lại thẻ`1`. Vì các quân bài bằng nhau không ghi điểm nên kết quả mong đợi là`0`. Công thức tạo ra`(1² - 1) / 3 = 0`, nên không cần có trường hợp đặc biệt nào. 

Vì`n = 2`, các thẻ là`1`Và`2`. Thẻ`1`không có thẻ nhỏ hơn, mang lại mức đóng góp dự kiến ​​là`0`. Thẻ`2`có một đối thủ nhỏ hơn trong số hai đối thủ có thể có, vì vậy đóng góp của nó là`2 * 1/2 = 1`. Câu trả lời cuối cùng là`1`. Điều này phát hiện những triển khai vô tình coi sự bình đẳng là một chiến thắng. 

Vì`n = 3`, những đóng góp là`0`,`2/3`, Và`2`, cho`8/3 = 2.6666666667`. Điều này kiểm tra quá trình chuyển đổi từ giá trị số nguyên sang giá trị mong đợi phân số và phát hiện từng lỗi một trong số lượng thẻ nhỏ hơn. 

Vì`n = 10^6`, công thức cho 

3 10 12 −1 ​ =333333333333. 

Kết quả lớn, nhưng các số nguyên có độ chính xác tùy ý của Python xử lý chính xác phép nhân trung gian. Không cần vòng lặp, mảng hoặc mô phỏng ngay cả ở kích thước đầu vào tối đa.
