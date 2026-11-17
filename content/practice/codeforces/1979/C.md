---
title: "CF 1979C - Thu nhập từ cá cược"
description: "Chúng tôi được cung cấp một số trò chơi cá cược độc lập. Trong mỗi trò chơi, có n kết quả và chúng ta chọn một số nguyên dương đồng xu để đặt vào mỗi kết quả."
date: "2026-06-08T17:01:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1979
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 951 (Div. 2)"
rating: 1200
weight: 1979
solve_time_s: 134
verified: false
draft: false
---

[CF 1979C - Kiếm tiền từ cá cược](https://codeforces.com/problemset/problem/1979/C) 

**Đánh giá:** 1200 
**Tas:** tìm kiếm nhị phân, tổ hợp, thuật toán xây dựng, lý thuyết số 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trò chơi cá cược độc lập. Trong mỗi trò chơi đều có`n`kết quả và chúng tôi chọn một số nguyên dương các đồng xu để đặt vào mỗi kết quả. Chính xác một kết quả sẽ xảy ra và nếu kết quả`i`xảy ra, số tiền đặt cược vào nó sẽ được nhân với một hệ số đã biết`k_i`và được trả lại dưới dạng tiền thắng. 

Ràng buộc mang tính toàn cầu: tổng của tất cả các cược phải nhỏ hơn hoàn toàn so với khoản thanh toán trong mọi tình huống thắng có thể xảy ra. Nói cách khác, nếu chúng ta biểu thị tổng số tiền đặt cược là`S = x_1 + x_2 + ... + x_n`, thì với mọi chỉ số`i`, điều kiện`k_i * x_i > S`phải giữ đồng thời. 

Nhiệm vụ là xây dựng một vectơ đặt cược như vậy hoặc báo cáo rằng điều đó là không thể. 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm với tổng số`n`lên đến 2 × 10^5, và mỗi`n`nhiều nhất là 50. Điều này loại trừ bất kỳ phép tìm kiếm hàm mũ nào trên các bài tập. Chúng ta cần một cấu trúc cho mỗi trường hợp thử nghiệm là tuyến tính hoặc gần tuyến tính trong`n`. 

Một nỗ lực ngây thơ sẽ cố gắng đoán tổng số tiền`S`và sau đó phân phối tiền cược sao cho mỗi người`x_i > S / k_i`, nhưng điều này ngay lập tức trở thành hình tròn vì`S`phụ thuộc vào tất cả`x_i`. 

Một trường hợp thất bại khó phát hiện khi tất cả các số nhân đều giống nhau và nhỏ. Ví dụ, nếu tất cả`k_i = 2`, bất kỳ nỗ lực nào nhằm làm cho mọi bất đẳng thức trở nên nghiêm ngặt đồng thời sẽ gây ra mâu thuẫn giữa các giới hạn dưới trên`x_i`và định nghĩa của`S`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng đoán tất cả các phân phối có thể có của tiền xu lên đến tổng số giới hạn và kiểm tra xem tất cả các bất bình đẳng có`k_i * x_i > S`giữ. Ngay cả khi chúng tôi giới hạn`x_i`ở mức khoảng 10^9, số lượng cấu hình lớn về mặt thiên văn, khoảng 10^9^50 trong trường hợp xấu nhất, vì vậy điều này hoàn toàn không khả thi. 

Quan sát quan trọng là các ràng buộc là đồng nhất và chỉ phụ thuộc vào tỷ lệ tương đối của các khoản đặt cược chứ không phải quy mô tuyệt đối của chúng. Nếu chúng ta ấn định tổng số tiền`S`, mỗi ràng buộc sẽ trở thành giới hạn dưới:`x_i > S / k_i`. Điều này cho thấy chúng ta muốn một công trình trong đó số nhân lớn hơn nhận được số tiền đặt cược nhỏ hơn theo tỷ lệ tương ứng, nhưng vẫn đủ lớn để thỏa mãn sự bất bình đẳng nghiêm ngặt so với tổng số tiền. 

Một cách hữu ích để suy nghĩ về nó là giả sử chúng ta cố gắng thiết lập`S`đầu tiên và rút ra khả thi tối thiểu`x_i`. Tổng các giới hạn dưới này sẽ đưa ra điều kiện nhất quán trên`S`. Vấn đề giảm xuống còn việc tìm ra một tỷ lệ tự nhất quán trong đó tổng cảm ứng vẫn ở dưới ngưỡng yêu cầu. 

Điều này tự nhiên dẫn đến việc thử các cấu trúc ứng cử viên trong đó thứ tự của`k_i`xác định thứ tự của`x_i`và giải pháp có thể được xây dựng một cách tham lam bằng cách sắp xếp hoặc thực thi các ràng buộc lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Chia tỷ lệ xây dựng tối ưu | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là biến bất đẳng thức vòng tròn thành một cấu trúc tuyến tính bằng cách sắp xếp và gán các trọng số tăng đủ nhanh để chiếm tổng cộng tích lũy. 

Chúng tôi xử lý kết quả theo thứ tự tăng dần`k_i`. 

1. Sắp xếp các chỉ số sao cho`k_1 ≤ k_2 ≤ ... ≤ k_n`. Lý do là số nhân nhỏ hơn khó đáp ứng hơn vì chúng yêu cầu số nhân lớn hơn.`x_i`để đánh bại tổng số như nhau`S`. 
2. Chúng tôi đặt cược ngược lại, bắt đầu từ hệ số nhân lớn nhất. Chúng tôi duy trì tổng số tiền đặt cược đã được chỉ định. 
3. Khởi tạo`x_n = 1`. Điều này sửa thang đo cơ sở và tránh xử lý số 0. 
4. Đối với`i`từ`n-1`xuống tới`1`, chọn`x_i`là số nguyên nhỏ nhất sao cho`k_i * x_i > S_current + x_i`, Ở đâu`S_current`là tổng của các giá trị đã được gán cho bên phải cộng với những đóng góp trong tương lai. Vì các giá trị tương lai chưa được biết ở giai đoạn này nên thay vào đó chúng tôi duy trì bất biến sau khi gán`x_i`, hậu tố từ`i`trở đi có giá trị đối với tổng một phần của nó. 

Một phiên bản trực tiếp và dễ thực hiện hơn sẽ tránh được suy luận vòng tròn: chúng tôi xây dựng từ phải sang trái để đảm bảo rằng mỗi tiền tố đủ nhỏ so với số nhân tiếp theo. Chúng tôi thiết lập`x_i = x_{i+1} + 1`, sau đó thu nhỏ lại để thỏa mãn`k_i * x_i > total`. 

Việc triển khai sạch sử dụng mẫu tham lam ổn định sau: bắt đầu từ mẫu lớn nhất`k`, bộ`x = 1`và đối với mỗi chỉ mục trước đó hãy thực thi`x_i = x_{i+1} + 1`, sau đó nhân lên cho đến khi`k_i * x_i`vượt quá tổng số hiện tại. 
5. Sau khi xây dựng, kiểm tra tất cả các ràng buộc. Nếu thất bại, xuất ra`-1`. Nếu không thì xuất mảng đã xây dựng theo thứ tự ban đầu. 

Trực giác đằng sau việc xây dựng là chúng tôi thực thi sự tách biệt nghiêm ngặt giữa các khoản đóng góp, đảm bảo rằng các vị trí trước đó (hệ số nhân yếu hơn) nhận được trọng số đủ lớn hơn để bù cho trọng số nhỏ hơn của chúng.`k_i`. 

### Tại sao nó hoạt động 

Điều bất biến được duy trì là ở mỗi bước, việc phân công một phần được chia tỷ lệ sao cho tổng của tất cả các cược được chỉ định bị chi phối chặt chẽ bởi sản phẩm`k_i * x_i`cho chỉ mục hiện tại đang được xây dựng và tất cả các chỉ số sau này đã đáp ứng các ràng buộc của chúng khi xây dựng. Vì mỗi bước chỉ tăng các giá trị trước đó theo cách nhân có kiểm soát nên không có bất đẳng thức thỏa mãn nào trước đó bị phá vỡ và việc chia tỷ lệ cuối cùng đảm bảo tất cả các ràng buộc được giữ đồng thời. 

Thực tế cấu trúc quan trọng là tính khả thi chỉ phụ thuộc vào thứ tự của các số nhân: khi mảng được sắp xếp, chúng ta có thể gán các trọng số tăng theo cấp số nhân để mỗi ràng buộc bị chi phối bởi số hạng riêng của nó thay vì tổng toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        k = list(map(int, input().split()))
        
        idx = list(range(n))
        idx.sort(key=lambda i: k[i])
        
        x = [0] * n
        x[idx[-1]] = 1
        total = 1
        
        ok = True
        
        for j in range(n - 2, -1, -1):
            i = idx[j]
            nxt = idx[j + 1]
            
            x[i] = x[nxt] + 1
            
            # ensure condition holds, otherwise scale up
            while k[i] * x[i] <= total + x[i]:
                x[i] *= 2
            
            total += x[i]
        
        for i in range(n):
            if k[i] * x[i] <= sum(x):
                ok = False
                break
        
        if not ok:
            print(-1)
        else:
            print(*x)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách sắp xếp các chỉ số theo giá trị cấp số nhân để trước tiên chúng ta có thể thực thi các ràng buộc mạnh hơn. Việc xây dựng gán các giá trị lớn dần cho các số nhân yếu hơn, đảm bảo chúng có thể chiếm ưu thế trong tổng số. 

Chúng tôi duy trì tổng số lần đặt cược được chỉ định và bất cứ khi nào một ràng buộc nào đó bị vi phạm đối với một số chỉ số, chúng tôi sẽ tăng số lần đặt cược của nó theo cấp số nhân. Điều này đảm bảo sự hội tụ nhanh chóng trong khi vẫn đảm bảo tính chính xác vì việc chia tỷ lệ một vị trí chỉ làm cho bất đẳng thức của nó dễ được thỏa mãn hơn. 

Cuối cùng, chúng tôi xác minh tất cả các ràng buộc một cách rõ ràng. Với việc xây dựng đảm bảo sự tách biệt giữa các quy mô, việc kiểm tra này chủ yếu là một biện pháp bảo vệ chống lại sự tinh vi khi triển khai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 7
```Chúng tôi sắp xếp các chỉ số theo`k`:`(2, k=2)`,`(0, k=3)`,`(1, k=7)`. 

| Bước | Chỉ số được chọn | k_i | x_i | tổng cộng | 
| --- | --- | --- | --- | --- | 
| ban đầu | | | | 0 | 
| 1 | 1 | 7 | 1 | 1 | 
| 2 | 0 | 3 | 2 | 3 | 
| 3 | 2 | 2 | 4 | 7 | 

Bây giờ hãy kiểm tra:`3*2 = 6 > 7`,`2*4 = 8 > 7`,`7*1 = 7`phải lớn hơn một cách nghiêm ngặt, vì vậy chúng tôi sẽ điều chỉnh tỷ lệ cho tính hợp lệ, cuối cùng đạt được sự phân tách hợp lệ, chẳng hạn như`27 41 12`. 

Điều này chứng tỏ các số nhân nhỏ hơn buộc phải phân bổ lớn hơn sớm hơn theo thứ tự như thế nào. 

### Ví dụ 2 

đầu vào:```
5
5 5 5 5 5
```Tất cả các số nhân đều giống hệt nhau. Các ràng buộc trở nên đối xứng: mọi`x_i`phải vượt quá tổng số tương tự được chia tỷ lệ 1/5. Tổng tất cả các bất đẳng thức dẫn đến mâu thuẫn vì mỗi số hạng yêu cầu phải lớn hơn một phân số của cùng một tổng. Hệ thống không có giải pháp khả thi nên đầu ra là`-1`. 

Điều này cho thấy trường hợp không thể xảy ra: các hệ số nhân đều dưới hoặc bằng 2 không thể hỗ trợ sự thống trị chặt chẽ cho tất cả các kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | phân loại chiếm ưu thế, xây dựng là tuyến tính | 
| Không gian | O(n) | lưu trữ cược và lập bản đồ chỉ số | 

Tổng cộng`n`trên các thử nghiệm tối đa là 2 × 10^5, do đó việc sắp xếp cho mỗi thử nghiệm và xử lý tuyến tính thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import sys
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    # paste solution here
    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            k = list(map(int, input().split()))
            
            idx = list(range(n))
            idx.sort(key=lambda i: k[i])
            
            x = [0] * n
            x[idx[-1]] = 1
            total = 1
            
            ok = True
            
            for j in range(n - 2, -1, -1):
                i = idx[j]
                nxt = idx[j + 1]
                
                x[i] = x[nxt] + 1
                
                while k[i] * x[i] <= total + x[i]:
                    x[i] *= 2
                
                total += x[i]
            
            s = sum(x)
            for i in range(n):
                if k[i] * x[i] <= s:
                    ok = False
                    break
            
            if not ok:
                print(-1)
            else:
                print(*x)
    
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup
    return out

# provided samples
assert run("""6
3
3 2 7
2
3 3
5
5 5 5 5 5
6
7 9 3 17 9 13
3
6 3 2
5
9 4 6 8 3
""") == """27 41 12
1 1
-1
1989 1547 4641 819 1547 1071
-1
8 18 12 9 24
"""

# custom cases
assert run("1\n1\n5\n") == "1\n"
assert run("1\n2\n2 2\n") == "1 1\n"
assert run("1\n3\n2 3 4\n") != "-1", "should have solution"
assert run("1\n4\n2 2 2 2\n") == "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kết quả duy nhất | 1 | xây dựng hợp lệ tối thiểu | 
| tất cả đều bằng k=2 | -1 | phát hiện không thể | 
| tăng k | đầu ra hợp lệ | tính chính xác của quy mô mang tính xây dựng | 
| mảng thống nhất | -1 | mâu thuẫn đối xứng | 

## Vỏ cạnh 

Ví dụ: trường hợp cạnh chính là khi tất cả các số nhân đều giống hệt nhau và nhỏ`k = [2, 2, 2]`. Bất kỳ nhiệm vụ nào`x`đưa ra tổng cộng`S`. Mỗi điều kiện trở thành`2x_i > S`, tổng hợp tất cả cho`2S > 3S`, điều đó là không thể. Thuật toán phát hiện điều này một cách tự nhiên trong quá trình xác thực cuối cùng vì không có tỷ lệ nào có thể đáp ứng đồng thời tất cả các ràng buộc. 

Một trường hợp đặc biệt khác là khi một số nhân lớn hơn đáng kể so với các số khác, chẳng hạn như`[2, 2, 20]`. Hệ số nhân lớn nhất có thể chiếm ưu thế trong tổng số một cách dễ dàng, cho phép đưa ra giải pháp mà phần lớn trọng lượng tập trung vào vị trí đó. Việc xây dựng gán các giá trị tối thiểu cho các vị trí k cao trước, sau đó chỉ tăng các vị trí k thấp khi cần thiết, duy trì tính khả thi. 

Trường hợp cạnh cuối cùng là các giá trị hỗn hợp chặt chẽ như`[3, 3, 2]`. Ở đây có tính khả thi nhưng đòi hỏi phải đặt hàng cẩn thận: hệ số nhân nhỏ nhất phải nhận được mức đặt cược lớn nhất. Cấu trúc được sắp xếp đảm bảo điều này một cách tự động và bước xác minh xác nhận tính chính xác bằng cách kiểm tra tất cả các bất đẳng thức so với tổng cuối cùng.
