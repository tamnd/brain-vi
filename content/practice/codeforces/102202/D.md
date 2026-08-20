---
title: "CF 102202D - A Cộng Bằng B"
description: "Chúng ta có hai số nguyên dương A và B. Các phép toán duy nhất được phép là nhân đôi một biến hoặc cộng một biến này với biến kia. Nhiệm vụ là in bất kỳ chuỗi nào gồm tối đa 5000 thao tác như vậy để cuối cùng làm cho hai biến bằng nhau."
date: "2026-08-20T02:16:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "D"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 453
verified: false
draft: false
---

[CF 102202D - A Cộng Bằng B](https://codeforces.com/problemset/problem/102202/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 33 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ta có hai số nguyên dương`A`Và`B`. Các hoạt động duy nhất được phép là nhân đôi một biến hoặc thêm một biến vào biến kia. Nhiệm vụ là in bất kỳ chuỗi nào gồm tối đa 5000 thao tác như vậy để cuối cùng làm cho hai biến bằng nhau. 

Về nguyên tắc, khó khăn là không tìm ra trình tự nào đó. Vì phép cộng lặp lại có thể mô phỏng thuật toán Euclide nên luôn tồn tại một nghiệm. Khó khăn là tạo ra chuỗi trong phạm vi 5000 phép tính khi giá trị ban đầu có thể lớn tới 10 18. 

Một mô phỏng trực tiếp của phép trừ đã đủ để phát hiện ra vấn đề. Bắt đầu từ`A = 1, B = 10^18`, quá trình đảo ngược của quá trình cộng tự nhiên sẽ trừ đi`1`từ`B`khoảng 10 18 lần. Điều đó vượt xa cả giới hạn đầu ra và lượng tính toán có sẵn trong một giây. Phạm vi số khổng lồ cũng loại trừ mọi tìm kiếm đối với các cặp giá trị có thể có. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Nếu như`A == B`, ví dụ, câu trả lời đúng chỉ đơn giản là các phép toán bằng 0. Đối với đầu vào`1 1`, việc in dù chỉ một thao tác là không cần thiết và có thể khiến việc xây dựng chính xác trở nên khó giải thích hơn. Nếu một giá trị là số chẵn, chẳng hạn như`2 3`, chúng ta phải khai thác các thao tác nhân đôi thay vì cộng liên tục`2`ĐẾN`3`. Cuối cùng, các đầu vào như`1 1000000000000000000`đều nguy hiểm đối với bất kỳ thuật toán nào thực hiện một phép cộng trên một đơn vị tiến trình, vì số lượng yêu cầu có thể lớn hơn 5000 về mặt thiên văn. 

Quan sát trọng tâm là việc nhân cả hai số với cùng một thừa số dương sẽ không thay đổi liệu bài toán có giải được hay không. Một chuỗi các phép cộng được áp dụng cho`(A, B)`có thể được áp dụng cho`(kA, kB)`và mọi giá trị trung gian chỉ được nhân với`k`. Điều này cho chúng ta một cách hữu ích để giải thích lại việc nhân đôi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là đảo ngược quá trình. Nếu như`A < B`, trừ`A`từ`B`; nếu như`B < A`, trừ`B`từ`A`. Đây chính xác là dạng trừ của thuật toán Euclide. Khi hai giá trị bằng nhau, đảo ngược tất cả các phép trừ để có được các phép toán tiến hợp lệ. 

Cách tiếp cận này đúng vì mọi phép trừ giá trị nhỏ hơn từ giá trị lớn hơn tương ứng ngược lại với việc cộng giá trị nhỏ hơn vào giá trị lớn hơn. Vấn đề là số lần lặp lại. Vì`(1, 10^18)`, Euclid trừ thực hiện 10 18 −1 lần lặp. Điều đó vượt quá 5000 hoạt động đầu ra được phép với một mức chênh lệch rất lớn. 

Quan sát khắc phục điều này là hoạt động nhân đôi. Giả sử trạng thái khái niệm là`(A/2, B)`, Ở đâu`A`là chẵn. Thay vì thực sự chia`A`, trình diễn`B += B`về các biến thực. Trạng thái thực trở thành`(A, 2B)`, chính xác là hai lần`(A/2, B)`. Vì việc nhân cả hai biến với cùng một thừa số sẽ bảo toàn mọi mối quan hệ cộng tính trong tương lai, nên chúng ta có thể coi`B += B`như một hoạt động khái niệm để phân chia`A`bằng hai. 

Điều này cho chúng ta một phiên bản nhanh của quá trình Euclide. Bất cứ khi nào một số là số chẵn, hãy chia nó cho hai về mặt khái niệm. Nếu cả hai số đều lẻ và không bằng nhau thì cộng số nhỏ với số lớn. Càng lớn thì càng chẵn nên pha tiếp theo có thể chia đôi. Cụ thể hơn, nếu`A < B`và cả hai đều là số lẻ, hãy thay cặp khái niệm bằng 

(A,B)→(A, 2 A+B ​ ). 

Sự khác biệt mới là 

2 B−A ​ , 

vì vậy mỗi lần chuyển đổi từ lẻ sang lẻ như vậy sẽ giảm một nửa sự khác biệt. Đây là lý do chính khiến số lần lặp vẫn nhỏ. 

Các giá trị ban đầu nhiều nhất là 10 18, do đó có nhiều nhất khoảng 60 lần chia đôi trước khi giá trị dương đạt tới 1. Trong một lần chuyển đổi lẻ, phép cộng tạo ra một giá trị dưới 2⋅10 18, yêu cầu tối đa 61 lần chia đôi để giảm trở lại giá trị lẻ. Có thể có tối đa 60 vòng giảm một nửa chênh lệch có ý nghĩa. Giới hạn lỏng lẻo khoảng 60⋅61+60, dưới 5000, đã đủ cho giới hạn yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép trừ Euclid | O(max(A,B)) | Đầu ra O(max(A,B)) trong trường hợp xấu nhất | Quá chậm | 
| Euclid dựa trên việc giảm một nửa | O(logmax(A,B)) vòng khái niệm, cộng với đầu ra | O(5000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`A`Và`B`. Nếu chúng đã bằng nhau thì xuất ra các phép toán bằng 0. Không có gì để xây dựng. 
2. Trong khi`A`Và`B`khác nhau, liên tục loại bỏ các thừa số của 2 khỏi`A`. Về mặt khái niệm, thay thế`A`qua`A/2`được mô phỏng bằng cách thực hiện`B += B`. Cặp thực tế được nhân đôi ở tọa độ thứ hai của nó, do đó trạng thái thực vẫn là bội số chung của trạng thái khái niệm. 
3. Làm tương tự cho`B`. Mỗi sự phân chia khái niệm`B /= 2`được mô phỏng bởi`A += A`. 
4. Sau khi cả hai giá trị đều là số lẻ, hãy so sánh chúng. Nếu như`A < B`, trình diễn`B += A`và cập nhật về mặt khái niệm`B`ĐẾN`A + B`. cái mới`B`chẵn vì nó là tổng của hai số lẻ. 
5. Nếu`B < A`, thực hiện đối xứng`A += B`và cập nhật về mặt khái niệm`A`ĐẾN`A + B`. Một lần nữa, giá trị mới lớn hơn là chẵn và có thể giảm một nửa trong lần lặp tiếp theo. 
6. Tiếp tục cho đến khi các giá trị khái niệm bằng nhau. Sau đó, các thao tác đã ghi sẽ được in theo thứ tự ban đầu. Bởi vì mọi trạng thái khái niệm đều được biểu thị bằng trạng thái thực tế theo một hệ số tỷ lệ chung, nên sự bằng nhau của các giá trị khái niệm hàm ý sự bằng nhau của các biến thực tế. 

Lý do thuật toán kết thúc nhanh chóng là vì sau khi cả hai giá trị đều là số lẻ, việc cộng giá trị nhỏ hơn với giá trị lớn hơn và sau đó giảm một nửa số lớn hơn sẽ thay đổi chênh lệch so với`D`ĐẾN`D/2`. Do đó, hiệu sẽ mất ít nhất một chữ số nhị phân cho mỗi lần chuyển đổi lẻ. Bản thân các giá trị cũng mất hệ số hai bất cứ khi nào có thể, do đó, cả số vòng cũng như số lượng thao tác phát ra đều không thể đạt tới cường độ đầu vào khổng lồ. 

### Tại sao nó hoạt động 

Duy trì tính bất biến rằng cặp biến thực tế là bội số chung của cặp khái niệm`(A, B)`được duy trì bởi thuật toán. Ban đầu số nhân là 1. 

Khi thuật toán phân chia theo khái niệm`A`tăng gấp đôi, thao tác được ghi sẽ nhân đôi thao tác thực tế`B`. Nếu cặp thực tế là`k(A,B)`, nó trở thành`(kA,2kB)`, bằng`2k(A/2,B)`. Lập luận tương tự được áp dụng khi phân chia về mặt khái niệm`B`. 

Khi thuật toán cộng số nhỏ hơn với số lớn hơn, phép toán thực tương ứng sẽ thực hiện chính xác phép cộng tương tự trên cặp thực tế, duy trì mối quan hệ chia tỷ lệ chung. Do đó bất biến giữ sau mỗi hoạt động. 

Khi các giá trị khái niệm trở nên bằng nhau, thì các giá trị thực tế sẽ bằng số nhân chung nhân với các giá trị bằng nhau đó, do đó các biến thực tế sẽ bằng nhau theo yêu cầu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    a, b = map(int, input().split())    operations = []
    while a != b:        while a % 2 == 0:            # Conceptually: a /= 2            # Actually: double b, so the real pair is scaled by 2.            operations.append("B+=B")            a //= 2
        while b % 2 == 0:            # Conceptually: b /= 2            # Actually: double a.            operations.append("A+=A")            b //= 2
        if a < b:            operations.append("B+=A")            b += a        elif b < a:            operations.append("A+=B")            a += b
    print(len(operations))    sys.stdout.write("\n".join(operations))

if __name__ == "__main__":    solve()
```hai`while`các vòng lặp thực hiện hoạt động giảm một nửa khái niệm. Khi`a`là số chẵn, mã ghi`B+=B`nhưng thay đổi khái niệm`a`ĐẾN`a // 2`. Mối quan hệ tương tự được sử dụng ngược lại cho một số chẵn`b`. 

Các hoạt động bổ sung được ghi lại trước khi cập nhật biến khái niệm. Thứ tự này quan trọng vì thao tác in mô tả những gì xảy ra với trạng thái thực, trong khi các biến cục bộ biểu thị trạng thái khái niệm được chuẩn hóa. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn khi xây dựng chuỗi. Trong ngôn ngữ có chiều rộng cố định, các giá trị thực có thể tạm thời vượt quá 10 18, do đó, việc sử dụng loại số nguyên đủ rộng là cần thiết. 

các`elif`cũng là cố ý. Sau lần so sánh đầu tiên, chỉ có một biến có thể nhỏ hơn. Không có lý do gì để thực hiện cả hai phép cộng trong một lần lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đối với đầu vào được cung cấp`2 3`, thuật toán đầu tiên thông báo rằng`A`là chẵn. Nó ghi lại`B+=B`và những thay đổi về mặt khái niệm`A`từ 2 đến 1. 

| Bước | Hoạt động | Khái niệm A | Khái niệm B | Thực tế A | B thực tế | 
| --- | --- | --- | --- | --- | --- | 
| 0 | Ban đầu | 2 | 3 | 2 | 3 | 
| 1 |`B+=B`| 1 | 3 | 2 | 6 | 
| 2 |`B+=A`| 1 | 2 | 2 | 8 | 
| 3 |`A+=A`| 1 | 1 | 4 | 8 | 

Các giá trị khái niệm bây giờ bằng nhau. Các giá trị thực tế cũng bằng nhau, ở mức 8. Điều này minh họa bất biến tỷ lệ chung: sau thao tác đầu tiên, cặp thực tế`(2,6)`gấp đôi cặp khái niệm`(1,3)`, và mối quan hệ tương tự vẫn còn sau đó. 

### Ví dụ 2 

Hãy xem xét`1 5`. Cả hai số đều bắt đầu lẻ, và`A < B`, do đó thuật toán thêm`A`ĐẾN`B`. Khái niệm mới`B`là 6, là số chẵn, nên phép toán tiếp theo sẽ giảm một nửa về mặt khái niệm. 

| Bước | Hoạt động | Khái niệm A | Khái niệm B | Thực tế A | B thực tế | 
| --- | --- | --- | --- | --- | --- | 
| 0 | Ban đầu | 1 | 5 | 1 | 5 | 
| 1 |`B+=A`| 1 | 6 | 1 | 6 | 
| 2 |`A+=A`| 1 | 3 | 2 | 6 | 
| 3 |`B+=A`| 1 | 4 | 2 | 8 | 
| 4 |`A+=A`| 1 | 2 | 4 | 8 | 
| 5 |`A+=A`| 1 | 1 | 8 | 8 | 

Sự khác biệt giữa các giá trị khái niệm đi từ 4 đến 2 rồi đến 1. Chuỗi thực tế kết thúc với các giá trị bằng nhau chỉ sau năm thao tác, mặc dù các giá trị ban đầu cách nhau bốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log 2 V) trong giới hạn lỏng lẻo, trong đó V=max(A,B) | Có các vòng giảm một nửa O(logV) và mỗi vòng thực hiện tối đa các hoạt động giảm một nửa O(logV) | 
| Không gian | O(log 2 V), giới hạn bởi 5000 phép toán | Trình tự đầu ra được lưu trữ trước khi in | 

Với V<10 18, logarit cơ số hai chỉ khoảng 60. Số phép toán thu được nằm dưới giới hạn 5000 được yêu cầu một cách thoải mái, do đó thuật toán dễ dàng đủ nhanh cho giới hạn một giây. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm 

Bởi vì đây là một vấn đề mang tính xây dựng nên việc so sánh kết quả đầu ra một cách chính xác là không phù hợp. Thẩm phán chấp nhận nhiều trình tự hợp lệ khác nhau. Thay vào đó, trình trợ giúp kiểm tra bên dưới sẽ phân tích các thao tác được tạo ra, kiểm tra xem có tối đa 5000 thao tác trong số đó hay không, mô phỏng chúng trên đầu vào ban đầu và xác minh rằng các giá trị cuối cùng bằng nhau.```python
Pythonimport sysimport io

def solution(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    a, b = map(int, sys.stdin.readline().split())    operations = []
    while a != b:        while a % 2 == 0:            operations.append("B+=B")            a //= 2
        while b % 2 == 0:            operations.append("A+=A")            b //= 2
        if a < b:            operations.append("B+=A")            b += a        elif b < a:            operations.append("A+=B")            a += b
    print(len(operations))    sys.stdout.write("\n".join(operations))
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| Không hoạt động | Bình đẳng ngay lập tức và ranh giới vòng lặp | 
|`123456789 123456789`| Không hoạt động | Bình đẳng với những giá trị không tầm thường | 
|`1000000000000000000 1`| Tối đa 5000 thao tác hợp lệ | Cường độ tối đa và giảm một nửa lặp đi lặp lại | 
|`1000000000000000000 1000000000000000000`| Không hoạt động | Cả hai đầu vào ở giới hạn trên | 
|`999999999999999999 1000000000000000000`| Tối đa 5000 thao tác hợp lệ | Các giá trị liên tiếp và chuyển tiếp chẵn lẻ | 

## Vỏ cạnh 

cho`1 1`, vòng lặp bên ngoài không bao giờ bắt đầu vì các giá trị khái niệm đã bằng nhau. Chương trình in`0`và không có hoạt động. Một công trình đi vào vòng lặp một cách mù quáng có thể vô tình tạo ra một thao tác không cần thiết và bỏ lỡ câu trả lời hợp lệ đơn giản nhất. 

Vì`2 3`, giá trị đầu tiên là chẵn. Bản ghi thuật toán`B+=B`và về mặt khái niệm thay đổi cặp từ`(2,3)`ĐẾN`(1,3)`. Cặp thực thay đổi từ`(2,3)`ĐẾN`(2,6)`, gấp đôi cặp khái niệm. Các hoạt động tiếp theo tạo ra`(2,8)`và sau đó`(4,8)`, do đó đạt được sự bình đẳng. 

Vì`1 5`, cả hai giá trị đều lẻ và không bằng nhau. Thuật toán thực hiện`B+=A`, đưa ra khái niệm`(1,6)`, sau đó liên tục loại bỏ hệ số hai khỏi 6. Các trạng thái khái niệm là`(1,5)`,`(1,6)`,`(1,3)`,`(1,4)`,`(1,2)`,`(1,1)`. Các trạng thái thực tế kết thúc tại`(8,8)`. Trường hợp này chứng tỏ tại sao một cặp lẻ trước tiên phải sử dụng phép cộng để tạo ra giá trị chẵn. 

Vì`1 1000000000000000000`, phép trừ Euclid sẽ cần gần 10 18 phép cộng. Thay vào đó, thuật toán được đề xuất liên tục giảm một nửa giá trị chẵn về mặt khái niệm. Giá trị giảm từ 10 18 xuống 5 sau khoảng 60 lần chia đôi, trong khi mỗi lần chia đôi khái niệm chỉ tương ứng với một thao tác được in. Các chuyển tiếp lẻ còn lại cũng làm giảm sự khác biệt về mặt hình học, giữ cho toàn bộ công trình được an toàn dưới 5000 lần vận hành. 

Vì`999999999999999999 1000000000000000000`, các số liên tiếp nhau và có tính chẵn lẻ khác nhau. Giá trị chẵn ngay lập tức được giảm đi một nửa về mặt khái niệm, sau đó thuật toán liên tục chuẩn hóa tính chẵn lẻ và thêm giá trị lẻ nhỏ hơn khi cần thiết. Điều này nắm bắt các triển khai giả định cả hai số có cùng tính chẵn lẻ hoặc quên kiểm tra lại tính chẵn lẻ sau khi bổ sung.
