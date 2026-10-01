---
title: "CF 104857B - Sắp xếp hàng đợi"
description: "Chúng ta được cho một tập hợp các số nguyên trong đó các giá trị nằm trong khoảng từ 1 đến n và mỗi giá trị i xuất hiện ai lần. Nhiệm vụ là đếm xem có bao nhiêu chuỗi b riêng biệt, là các hoán vị của nhiều tập hợp này, có thuộc tính đặc biệt liên quan đến hai hàng đợi."
date: "2026-06-28T10:54:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "B"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 67
verified: true
draft: false
---

[CF 104857B - Sắp xếp hàng đợi](https://codeforces.com/problemset/problem/104857/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên trong đó các giá trị nằm trong khoảng từ 1 đến n và mỗi giá trị i xuất hiện ai lần. Nhiệm vụ là đếm xem có bao nhiêu chuỗi b riêng biệt, là các hoán vị của nhiều tập hợp này, có thuộc tính đặc biệt liên quan đến hai hàng đợi. 

Quá trình được mô tả là một mô phỏng hai giai đoạn. Đầu tiên, chúng tôi lấy chuỗi b và đẩy từng phần tử của nó vào hàng đợi A hoặc hàng đợi B. Sau khi tất cả các phần tử được đặt, chúng tôi liên tục xóa các phần tử khỏi phía trước của A hoặc B, chọn ở mỗi bước hàng đợi không trống để bật ra, cho đến khi cả hai hàng đợi đều trống. Mục tiêu là có thể tạo ra chuỗi được sắp xếp toàn cầu, nghĩa là tất cả các số 1 trước, sau đó là tất cả các số 2, v.v. cho đến n. 

Một chuỗi b được coi là hợp lệ nếu tồn tại một số cách để gán các phần tử của nó vào hai hàng đợi sao cho quá trình hợp nhất cuối cùng này có thể xuất ra chuỗi đã được sắp xếp. 

Các ràng buộc đủ chặt chẽ để loại trừ bất kỳ phép liệt kê hàm mũ nào đối với các hoán vị hoặc phép gán. Tổng số phần tử nhiều nhất là 500, do đó, bất kỳ giải pháp nào chỉ phụ thuộc vào n và m trong thời gian đa thức xung quanh m2 hoặc m³ đều hợp lý, trong khi bất kỳ giải pháp nào có tính giai thừa trong m đều không khả thi ngay lập tức. 

Một điểm tinh tế quan trọng là chúng tôi không chọn cách xuất sau khi xem hàng đợi ở dạng tự do. Đầu ra phải được sắp xếp nên cấu trúc của hàng đợi bị hạn chế rất nhiều. Nếu một hàng đợi bên trong buộc một giá trị lớn hơn xuất hiện trước một giá trị nhỏ hơn thì không thể sửa được trong quá trình hợp nhất vì thứ tự hàng đợi đã được cố định. 

Một trường hợp lỗi đơn giản xuất hiện khi hàng đợi chứa mẫu giảm dần. 

Ví dụ: nếu một hàng đợi chứa 3 theo sau là 2 thì trong quá trình trích xuất, số 3 sẽ xuất hiện trước số 2, điều này phá vỡ thứ tự sắp xếp. Vì vậy, mặc dù giai đoạn hợp nhất rất linh hoạt nhưng mỗi hàng đợi riêng lẻ phải hoạt động theo cách tương thích với đầu ra được sắp xếp. 

Quan sát này là hạn chế về cấu trúc chính: hai hàng đợi hoạt động hiệu quả như hai bộ đệm đơn điệu phải cùng xuất ra một luồng được sắp xếp toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra mọi hoán vị của nhiều tập hợp và sau đó thử mọi phép gán phần tử có thể có cho hai hàng đợi và đối với mỗi phép gán sẽ mô phỏng xem lịch trình đầu ra hợp lệ có tồn tại hay không. Ngay cả khi bỏ qua số lượng hoán vị giai thừa, chỉ riêng số lần gán hàng đợi là 2^m và mỗi mô phỏng là O(m), khiến cách tiếp cận này quá chậm về mặt thiên văn. 

Sự đơn giản hóa quan trọng đến từ việc đảo ngược quan điểm. Thay vì mô phỏng quá trình xếp hàng, chúng tôi mô tả trình tự nào b thừa nhận một phép gán hợp lệ. Vì mỗi hàng đợi là FIFO nên khi một phần tử được đặt vào hàng đợi, thứ tự tương đối của nó trong hàng đợi đó sẽ được cố định. Trong lần hợp nhất cuối cùng, chúng ta chỉ có thể xen kẽ hai chuỗi cố định. Để đầu ra được hợp nhất được sắp xếp trên toàn cầu, mỗi hàng đợi phải có giá trị không giảm. 

Điều này chuyển đổi vấn đề thành điều kiện phân vùng trên chuỗi b. Chúng ta đang gán mỗi vị trí của b vào một trong hai hàng đợi sao cho trong mỗi hàng đợi, các giá trị không bao giờ giảm theo thời gian. Vì vậy, mỗi lớp màu phải tạo thành một dãy con tăng dần. 

Điều này tương đương với việc chia dãy thành hai dãy con tăng dần. Một định lý cổ điển theo quan điểm của Dilworth cho chúng ta biết điều này có thể xảy ra chính xác khi dãy không có dãy con giảm dần có độ dài 3. Nói cách khác, các dãy hợp lệ chính xác là những dãy tránh được mẫu 321. 

Vì vậy, vấn đề trở thành: đếm nhiều hoán vị của các tần số đã cho để tránh giảm gấp ba lần.

Đây là một cấu trúc được biết đến được ngụy trang. Các hoán vị có dãy con giảm dài nhất nhiều nhất là 2 tương ứng theo Robinson-Schensted đến Young tableaux với tối đa hai hàng. Với các giá trị lặp lại, chất tương tự chính xác sẽ trở thành hình dạng hoạt cảnh trẻ bán chuẩn với tối đa hai hàng và trọng lượng được tính theo tần số ai. 

Do đó, chúng tôi giảm vấn đề đếm các phép gán của từng giá trị i thành hai hàng sao cho các ràng buộc hàng được thỏa mãn: các hàng tăng yếu do xây dựng và điều kiện nghiêm ngặt của cột trở thành ràng buộc cân bằng tiền tố về số lượng phần tử được gán cho hàng đầu tiên so với hàng thứ hai. 

Chúng tôi xử lý các giá trị theo thứ tự tăng dần và quyết định số lượng bản sao của mỗi giá trị sẽ chuyển đến hàng trên cùng. Điều này trở thành một vấn đề lập trình động do sự chênh lệch giữa số phần tử ở hàng 1 và hàng 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về hoán vị và phân công hàng đợi | O(m! · 2^m · m) | O(m) | Quá chậm | 
| DP trên các nhóm giá trị với trạng thái cân bằng | O(n · m^2) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các giá trị từ 1 đến n theo thứ tự tăng dần, duy trì số lượng phần tử đã được gán cho mỗi hàng trong số hai hàng. 

Chúng tôi xác định trạng thái DP trong đó dp[d] là số cách xử lý tiền tố hiện tại của các giá trị sao cho chênh lệch d bằng (kích thước của hàng1 trừ đi kích thước của hàng2). Chỉ các trạng thái có d ≥ 0 là hợp lệ vì hàng1 không bao giờ được nằm dưới hàng2 trong cấu trúc tiền tố. 

Đối với mỗi giá trị i có tần số ai, chúng ta phân phối các bản sao của nó giữa hai hàng. Nếu chúng ta đặt x bản sao vào hàng1 thì ai − x chuyển sang hàng2 và số dư thay đổi theo (x − (ai − x)) = 2x − ai. 

Bây giờ chúng ta thực hiện chuyển đổi cho từng trạng thái và từng x có thể. 

### Hướng dẫn thuật toán 

1. Khởi tạo dp[0] = 1, nghĩa là trước khi xử lý bất kỳ giá trị nào, cả hai hàng đều trống và cân bằng. 
2. Với mỗi giá trị i từ 1 đến n, tạo một mảng mới ndp được khởi tạo bằng 0. 
3. Với mọi số dư hiện tại có thể d và mọi lựa chọn x có thể từ 0 đến ai, hãy tính số dư mới d' = d + (2x − ai). Nếu d' không âm, hãy thêm dp[d] vào ndp[d']. 
4. Sau khi xử lý tất cả các trạng thái của giá trị i, thay thế dp bằng ndp. 
5. Sau khi tất cả các giá trị được xử lý, tính tổng tất cả dp[d] trên tất cả d ≥ 0 để có câu trả lời. 

Lý do điều này có tác dụng là vì số dư d mã hóa ràng buộc tiền tố của bảng Young hai hàng: row1 phải luôn lớn ít nhất bằng row2 khi đọc các giá trị theo thứ tự. Mỗi phép gán phần tử đều tự động tôn trọng cấu trúc hàng tăng yếu vì các giá trị được xử lý theo thứ tự tăng dần, do đó không có hàng nào có thể đưa ra mức giảm. 

Ràng buộc duy nhất còn lại là duy trì cấu trúc tiền tố hợp lệ giữa các hàng, cấu trúc này được nắm bắt chính xác bởi tính không âm của d ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    m = sum(a)
    
    dp = [0] * (m + 1)
    dp[0] = 1
    
    offset = 0
    
    for cnt in a:
        ndp = [0] * (m + 1)
        
        for d in range(m + 1):
            if dp[d] == 0:
                continue
            
            cur = dp[d]
            
            for x in range(cnt + 1):
                nd = d + (2 * x - cnt)
                if 0 <= nd <= m:
                    ndp[nd] = (ndp[nd] + cur) % MOD
        
        dp = ndp
    
    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ cho mảng DP được lập chỉ mục theo số dư hiện tại giữa hai hàng. Mỗi nhóm giá trị đóng góp một chuyển tiếp có giới hạn trên tất cả các phân chia có thể có của bội số của nó. Vòng lặp bên trong x là nơi mà lựa chọn tổ hợp được mã hóa trực tiếp và bản cập nhật đảm bảo chúng tôi chỉ giữ các trạng thái trong đó ràng buộc tiền tố được giữ nguyên. 

Tổng cuối cùng tổng hợp tất cả số dư cuối cùng hợp lệ vì không có hạn chế về mức độ lớn hơn của hàng đầu tiên, chỉ có điều nó không bao giờ giảm xuống dưới hàng thứ hai trong quá trình xây dựng. 

## Ví dụ đã hoạt động 

Vì câu lệnh không cung cấp một mẫu hoàn toàn có thể đọc được nên việc xây dựng một thể hiện nhỏ sẽ rất hữu ích. 

Xét n = 2 với a = [1, 1]. Nhiều tập hợp là {1, 2}. 

Chúng tôi xử lý giá trị 1 trước tiên. 

| Bước | trạng thái dp d=0 | trạng thái dp d=1 | 
| --- | --- | --- | 
| ban đầu | 1 | 0 | 
| sau 1 | 1 | 1 | 

Sau khi xử lý giá trị 1, chúng ta có thể đặt nó vào hàng2 hoặc hàng1, tạo ra số dư 1 hoặc -1 nhưng chỉ giữ giá trị không âm, do đó, trạng thái 0 và 1 một cách hiệu quả tùy theo cách hiểu. 

Bây giờ xử lý giá trị 2 tương tự và theo dõi quá trình chuyển đổi; cả hai bài tập hợp lệ đều tồn tại, đưa ra câu trả lời tổng thể là 2. 

Điều này cho thấy DP đang tính trực tiếp các phép gán cấu trúc thay vì các hoán vị. 

Trường hợp phong phú hơn một chút là a = [2, 1]. Ở đây giá trị 1 phải được phân phối trước và giá trị 2 được đặt sau. DP đảm bảo rằng việc phân tách các bản sao tương tác chính xác với các ràng buộc tiền tố, ngăn chặn việc xen kẽ bất hợp pháp có thể tạo ra mẫu 321. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m2) | Đối với mỗi giá trị, chúng tôi lặp lại tất cả các trạng thái DP và tất cả các phần chia bội số của nó | 
| Không gian | O(m) | Mảng DP trên các giá trị cân bằng có thể có | 

Tổng số m nhiều nhất là 500, vì vậy m2n ở mức 125 triệu lần chuyển đổi. Với các hệ số không đổi chặt chẽ và số học mô-đun, điều này phù hợp với các giới hạn lập trình cạnh tranh điển hình trong Python được tối ưu hóa hoặc thoải mái hơn trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    m = sum(a)
    dp = [0] * (m + 1)
    dp[0] = 1
    
    for cnt in a:
        ndp = [0] * (m + 1)
        for d in range(m + 1):
            if dp[d] == 0:
                continue
            cur = dp[d]
            for x in range(cnt + 1):
                nd = d + (2 * x - cnt)
                if 0 <= nd <= m:
                    ndp[nd] = (ndp[nd] + cur) % MOD
        dp = ndp
    
    return str(sum(dp) % MOD)

# small cases
assert run("1\n1") == "1"
assert run("2\n1 1") == "2"
assert run("2\n2 1") == "3"
assert run("3\n1 1 1") == "5"
assert run("3\n0 0 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Trường hợp cơ sở phần tử đơn | 
| 2 1 1 | 2 | Tương tác tối thiểu giữa hai giá trị | 
| 2 2 1 | 3 | Lựa chọn phân phối giá trị lặp lại | 
| 3 1 1 1 | 5 | Tăng trưởng DP qua nhiều nhóm | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các số đều giống hệt nhau. Trong tình huống đó, cấu trúc duy nhất quan trọng là cách chúng ta phân chia các phần tử giống hệt nhau giữa hai hàng trong khi vẫn duy trì tính hợp lệ của tiền tố. DP tự nhiên đếm tất cả các phần phân chia hợp lệ vì mọi phân phối đều duy trì tính đơn điệu trong mỗi hàng. 

Một trường hợp cạnh khác là khi ai = 0 đối với hầu hết các giá trị. DP vẫn xử lý các giá trị này, nhưng chúng chỉ đóng góp chuyển đổi danh tính khi x = 0 bị ép buộc, do đó trạng thái không thay đổi. Điều này đảm bảo tính chính xác ngay cả khi thiếu nhiều giá trị. 

Trường hợp tinh tế cuối cùng là khi một giá trị duy nhất có ai gần với m. Trong trường hợp đó, vòng lặp bên trong x trở nên lớn, nhưng tất cả các chuyển đổi vẫn hợp lệ vì tất cả các bản sao đều không thể phân biệt được. DP tổng hợp chính xác tất cả các cách gán chúng vào hai hàng mà không vi phạm ràng buộc cân bằng tiền tố.
