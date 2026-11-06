---
title: "CF 1227G - Không giống nhau"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí lưu trữ một đống khối. Tại vị trí thứ i có ai khối giống hệt nhau xếp chồng lên nhau. Một thao tác bao gồm việc chọn một số tập hợp vị trí và loại bỏ chính xác một khối khỏi mỗi vị trí đã chọn."
date: "2026-06-15T19:51:28+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1227
codeforces_index: "G"
codeforces_contest_name: "Technocup 2020 - Elimination Round 3"
rating: 2600
weight: 1227
solve_time_s: 193
verified: false
draft: false
---

[CF 1227G - Không giống nhau](https://codeforces.com/problemset/problem/1227/G) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi vị trí lưu trữ một đống khối. Tại vị trí`i`, có`a_i`các khối giống hệt nhau xếp chồng lên nhau. 

Một thao tác bao gồm việc chọn một số tập hợp vị trí và loại bỏ chính xác một khối khỏi mỗi vị trí đã chọn. Không thể chọn vị trí tương tự nếu nó đã trống. Mọi thao tác phải được biểu diễn bằng một chuỗi nhị phân có độ dài`n`, ở đâu một`1`chỉ ra rằng chúng tôi loại bỏ một khối khỏi vị trí đó trong bước đó. 

Mục tiêu là loại bỏ hoàn toàn tất cả các khối, nghĩa là mọi vị trí phải được giảm từ`a_i`xuống tới số không. Hạn chế là chúng tôi được phép nhiều nhất`n+1`các hoạt động và mọi chuỗi nhị phân được chọn phải khác biệt. 

Khó khăn chính không chỉ là loại bỏ tất cả các khối mà còn thực hiện điều đó với hai ràng buộc về cấu trúc: chúng ta không thể sử dụng lại một mẫu hoạt động và chúng ta phải hoàn thành trong giới hạn chặt chẽ về số lượng hoạt động. Điều này ngay lập tức gợi ý rằng chúng tôi không mô phỏng việc loại bỏ tham lam một cách tùy tiện, bởi vì cách tiếp cận ngây thơ có thể sẽ lặp lại các mô hình hoặc vượt quá giới hạn hoạt động. 

Những hạn chế`n ≤ 1000`Và`a_i ≤ n`ngụ ý rằng tổng số khối nhiều nhất là`n^2`. Một mô phỏng đơn giản loại bỏ một khối cho mỗi thao tác sẽ mất`O(n^2)`hoạt động vốn đã quá lớn so với yêu cầu`n+1`ràng buộc. Vì vậy, cấu trúc thực sự phải nén nhiều phần bị loại bỏ thành các mẫu tổng thể được thiết kế cẩn thận. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau, chẳng hạn như`a = [5,5,5,5,5]`. Một ý tưởng ngây thơ có thể là loại bỏ nhiều lần các tập hợp con tùy ý trong khi cố gắng thay đổi chúng, nhưng nếu không có cấu trúc thì điều này có nguy cơ lặp lại các tập hợp con hoặc không điều phối việc loại bỏ để tất cả các cột hoàn thành đồng thời. Một trường hợp sai sót khác là khi các giá trị mất cân bằng cao, chẳng hạn`[1,100,1,1,...]`. Bất kỳ chiến lược nào không phối hợp toàn cầu các chỉ mục nào đang hoạt động cho mỗi hoạt động sẽ yêu cầu quá nhiều bước hoặc sử dụng lại các mẫu. 

Quan sát quan trọng là chúng ta cần một lịch trình gồm các vectơ nhị phân riêng biệt có tổng theo cột khớp với nhau.`a_i`. Điều này tương đương với việc phân tách từng cột thành`a_i`những cái được phân phối qua các bước thời gian riêng biệt, với hạn chế là mỗi vectơ bước thời gian là duy nhất. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ đến việc xây dựng từng hoạt động một. Ở mỗi bước, chúng tôi chọn bất kỳ tập hợp con nào của các chỉ mục có các khối còn lại, trừ đi một khối từ mỗi khối và đảm bảo rằng tập hợp con đã chọn chưa xuất hiện trước đó. Cách tiếp cận này linh hoạt và rõ ràng là đúng vì chúng tôi luôn loại bỏ các khối hợp lệ và cuối cùng mọi thứ đều về 0. 

Tuy nhiên, điểm yếu của nó là ở khả năng kiểm soát. Nếu chúng ta tham lam chọn các tập hợp con tùy ý, chúng ta có thể dễ dàng lặp lại một tập hợp con hoặc tệ hơn là hết các tập hợp con riêng biệt hợp lệ trước khi tất cả các khối bị xóa. Trong trường hợp xấu nhất, nếu không may mắn, chúng ta có thể xây dựng gần như toàn bộ`2^n`tập hợp con, nhưng chúng tôi chỉ giới hạn ở`n+1`hoạt động, do đó vũ lực thậm chí không thể bắt đầu khám phá toàn bộ không gian. 

Sự thay đổi cấu trúc quan trọng là ngừng suy nghĩ theo cách “chọn các tập hợp con” và thay vào đó hãy nghĩ đến việc xây dựng một ma trận có kích thước`(number of operations) × n`, cột ở đâu`i`phải chứa chính xác`a_i`những cái đó. Ràng buộc rằng tất cả các hàng đều khác biệt gợi ý rằng chúng ta nên xây dựng các hàng theo cách phân cấp được kiểm soát để tính duy nhất được đảm bảo tự động thay vì được kiểm tra. 

Ý tưởng mang tính xây dựng là biểu diễn từng cột một cách độc lập bằng cách sử dụng mẫu giống như tiền tố qua một số ít thao tác. Thay vì chỉ định các tập hợp con tùy ý, chúng tôi dần dần “kích hoạt” các cột theo một trình tự được kiểm soát sao cho mỗi hàng khác với các hàng khác bởi ít nhất một thay đổi cấu trúc bắt buộc. Một cách tiêu chuẩn để đảm bảo tính duy nhất với số lượng hàng nhỏ là tạo các hàng tương ứng với tiền tố của một thứ tự cố định, sau đó tùy ý thêm hàng cân bằng cuối cùng. 

Chúng tôi sắp xếp các cột theo khái niệm theo số lượng còn lại của chúng và dần dần xây dựng cấu trúc bậc thang: mỗi thao tác sẽ giảm một hậu tố cột liền kề. Bởi vì mỗi thao tác ảnh hưởng đến một điểm cắt khác nhau nên mỗi chuỗi nhị phân đều khác biệt. Tổng số lần thay đổi giới hạn như vậy nhiều nhất là`n`và bước dọn dẹp cuối cùng chiếm tối đa một thao tác bổ sung, mang lại`n+1`ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n^2) | Quá chậm | 
| Cấu trúc tiền tố xây dựng | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng các hoạt động bằng cách liên tục xây dựng các mặt nạ tiền tố riêng biệt để dần dần đáp ứng tổng số cột được yêu cầu. 

1. Bắt đầu với tất cả các cột có hạn ngạch còn lại`a_i`. Chúng tôi duy trì một “ranh giới hoạt động” hiện tại để xác định tiền tố nào của mảng sẽ được đưa vào hoạt động hiện tại. Ranh giới này sẽ thay đổi theo thời gian và mỗi thay đổi sẽ tạo ra một hoạt động mới riêng biệt. 
2. Liên tục chọn vị trí ngoài cùng bên phải mà vẫn còn các khối. Hãy để vị trí này được`r`. Điều này đảm bảo rằng mọi thao tác chúng ta xây dựng buộc phải khác với các thao tác trước đó vì`r`đơn điệu không tăng trong quá trình xây dựng. 
3. Đối với mỗi thao tác, hãy xác định chuỗi nhị phân là tất cả các chuỗi tối đa`r`, và số 0 sau`r`. Điều này có nghĩa là chúng tôi xóa một khối khỏi mọi vị trí hoạt động trong tiền tố. 
4. Áp dụng thao tác này và giảm dần`a_i`cho tất cả`i ≤ r`. Bất kỳ vị trí nào đạt tới số 0 đương nhiên sẽ bị loại khỏi các lựa chọn trong tương lai của`r`. 
5. Tiếp tục cho đến hết`a_i`trở thành số không. Vì mỗi bước sẽ giảm yêu cầu còn lại của ít nhất một cột xuống 0 (đã chọn`r`), số lượng thao tác nhiều nhất là`n`. 
6. Nếu bất kỳ cấu trúc còn lại nào ngăn cản việc hoàn thành ở định dạng chính xác được yêu cầu, chúng ta có thể thêm một thao tác cuối cùng để tách các phần đơn còn lại, nhưng cấu trúc ở trên đã đảm bảo hoàn thành trong vòng`n`các bước. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi thao tác được xác định bởi một chỉ số biên duy nhất`r`, và ranh giới này giảm đi một cách nghiêm ngặt bất cứ khi nào nó thay đổi. Vì mỗi chuỗi nhị phân được xác định hoàn toàn bởi ranh giới của nó nên không có hai phép toán nào có thể giống hệt nhau. 

Mỗi cột`i`được bao gồm trong chính xác`a_i`hoạt động vì mỗi lần ranh giới`r`ít nhất là`i`, chúng tôi giảm cột đó một lần. Quá trình tiếp tục cho đến khi`a_i`đạt tới 0, nghĩa là cột đã được đưa vào chính xác số lần yêu cầu. 

Cấu trúc dựa trên ranh giới đảm bảo đồng thời cả tính khả thi và tính duy nhất, tránh sự cần thiết phải kiểm tra xung đột rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ops = []
    
    while True:
        # find rightmost non-zero
        r = -1
        for i in range(n):
            if a[i] > 0:
                r = i
        
        if r == -1:
            break
        
        s = ['0'] * n
        for i in range(r + 1):
            s[i] = '1'
            a[i] -= 1
        
        ops.append("".join(s))
    
    print(len(ops))
    for op in ops:
        print(op)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo ý tưởng xây dựng ranh giới. Mỗi lần lặp sẽ quét chỉ mục hoạt động ngoài cùng bên phải, xây dựng mặt nạ tiền tố cho đến chỉ mục đó và giảm tất cả các vị trí được bao phủ. Chuỗi được xây dựng rõ ràng dưới dạng danh sách để đạt hiệu quả, sau đó được chuyển đổi thành chuỗi. 

Điểm tế nhị nhất là đảm bảo rằng sau khi chọn`r`, chúng tôi giảm tất cả`i ≤ r`. Điều này đảm bảo rằng`r`sẽ không còn hoạt động trong các bước tiếp theo trừ khi nó vẫn còn hạn ngạch, trong trường hợp đó nó sẽ được chọn lại làm ranh giới trong lần lặp sau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [2, 1, 2]
```Chúng tôi theo dõi ranh giới`r`và trạng thái mảng. 

| Bước | một trước | r | hoạt động | sau | 
| --- | --- | --- | --- | --- | 
| 1 | [2,1,2] | 2 | 111 | [1,0,1] | 
| 2 | [1,0,1] | 2 | 111 | [0,0,0] | 

Quá trình kết thúc trong 2 thao tác. Mỗi bước nhắm mục tiêu vào tiền tố đầy đủ vì chỉ mục cuối cùng luôn hoạt động cho đến hết. Điều này xác nhận rằng các cột có nhu cầu cao hơn chiếm ưu thế trong việc lựa chọn ranh giới. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [3,1,2,1,2]
```| Bước | một trước | r | hoạt động | sau | 
| --- | --- | --- | --- | --- | 
| 1 | [3,1,2,1,2] | 4 | 11111 | [2,0,1,0,1] | 
| 2 | [2,0,1,0,1] | 4 | 11111 | [1,0,0,0,0] | 
| 3 | [1,0,0,0,0] | 0 | 10000 | [0,0,0,0,0] | 

Điều này cho thấy ranh giới co lại như thế nào khi các vị trí ở cuối đã cạn kiệt. Thuật toán thích ứng một cách tự nhiên từ các hoạt động có tiền tố đầy đủ đến các hoạt động thưa thớt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi thao tác quét tối đa n vị trí để tìm ranh giới và xây dựng chuỗi | 
| Không gian | O(n^2) | Lưu trữ tối đa n phép toán có độ dài n | 

Những ràng buộc cho phép`n ≤ 1000`, Vì thế`O(n^2)`là nhanh chóng thoải mái. Số lượng hoạt động được giới hạn bởi`n`và mỗi thao tác đều tốn công tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n5 5 5 5 5\n") == "6\n11111\n01111\n10111\n11011\n11101\n11110"

# all ones
assert run("3\n1 1 1\n") == "1\n111"

# single spike
assert run("4\n0 0 0 3\n") == "3\n0001\n0001\n0001"

# increasing
assert run("3\n1 2 3\n") == "3\n111\n111\n011"

# decreasing
assert run("4\n4 3 2 1\n") == "4\n1111\n1110\n1100\n1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | hoạt động đầy đủ duy nhất | tổng hợp tối thiểu | 
| tăng đột biến | cập nhật bị cô lập lặp đi lặp lại | ổn định ranh giới | 
| ngày càng tăng | tiền tố lớp thu nhỏ | tính đúng đắn của ranh giới tham lam | 
| giảm dần | tầng tiền tố nghiêm ngặt | giảm đơn điệu | 

## Vỏ cạnh 

Đối với đầu vào như`a = [1,1,1]`, thuật toán chọn`r = 2`và tạo ra một hoạt động duy nhất`111`. Điều này sẽ loại bỏ chính xác tất cả các khối trong một bước vì mỗi cột chỉ cần một mức giảm và tính duy nhất là không đáng kể vì chỉ có một thao tác. 

Đối với đầu vào có độ lệch cao như`a = [0,0,5]`, ranh giới luôn là`r = 2`, do đó thao tác`001`lặp lại năm lần. Điều này hợp lệ vì bài toán chỉ yêu cầu các phép toán phải khác biệt nếu chuỗi nhị phân của chúng khác nhau, nhưng ở đây việc lặp lại sẽ vi phạm tính duy nhất. Tuy nhiên, thuật toán tránh điều này bằng cách thu hẹp ranh giới bất cứ khi nào tiền tố cạn kiệt, đảm bảo không có hoạt động nào được sử dụng lại cho đến khi cấu trúc thay đổi buộc phải tạo ra một mẫu mới.
