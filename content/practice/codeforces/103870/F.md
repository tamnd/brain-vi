---
title: "CF 103870F - Nhân bản"
description: "Chúng ta được cung cấp một dòng vị trí có độ dài nhất định, cùng với một số ràng buộc, mỗi ràng buộc mô tả một đoạn liền kề. Nhiệm vụ là quyết định xem có thể gán hai loại ký hiệu trên toàn bộ dòng sao cho tất cả các ràng buộc đều được thỏa mãn hay không."
date: "2026-07-02T07:45:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "F"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 44
verified: true
draft: false
---

[CF 103870F - Nhân bản](https://codeforces.com/problemset/problem/103870/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng vị trí có độ dài nhất định, cùng với một số ràng buộc, mỗi ràng buộc mô tả một đoạn liền kề. Nhiệm vụ là quyết định xem có thể gán hai loại ký hiệu trên toàn bộ dòng sao cho tất cả các ràng buộc đều được thỏa mãn hay không. 

Có một yêu cầu chung về cấu trúc: hai biểu tượng phải xuất hiện với số lượng bằng nhau trên toàn bộ cấu trúc. Điều này đã ngụ ý rằng tổng độ dài phải chẵn, vì chuỗi được chia đều cho hai loại. 

Mỗi khoảng ràng buộc áp đặt một điều kiện trên phân đoạn mà nó bao trùm. Tương tác ẩn mấu chốt là các điều kiện khoảng này không độc lập với hạn chế chẵn lẻ toàn cục. Điều quan trọng cuối cùng là liệu các ràng buộc có gây ra bất kỳ sự mất cân bằng cục bộ nào giữa các vị trí chẵn và lẻ trong một khoảng hay không. 

Các ràng buộc trở nên có ý nghĩa thông qua tính chẵn lẻ. Nếu chúng ta xem xét bất kỳ đoạn nào có độ dài lẻ, nó nhất thiết phải chứa nhiều hơn một vị trí của một chẵn lẻ (chỉ số chẵn hoặc lẻ) so với đoạn kia. Sự bất đối xứng đó xung đột trực tiếp với yêu cầu hai biểu tượng phải được cân bằng trong mọi cách giải thích hợp lệ nhằm tôn trọng cấu trúc toàn cầu. 

Từ góc độ này, vấn đề giảm xuống còn việc kiểm tra xem tập hợp các khoảng có mâu thuẫn với sự cân bằng chẵn lẻ hay liệu chúng ta luôn có thể xây dựng một mô hình xen kẽ hợp lệ hay không. 

### Vỏ cạnh 

Một cách tiếp cận đơn giản có thể cố gắng xây dựng một cách rõ ràng một phép gán hợp lệ và kiểm tra tất cả các ràng buộc, nhưng cách này không thành công với các đầu vào lớn vì cấu trúc quá linh hoạt và không gian xây dựng có tính hàm mũ. 

Ví dụ: hãy xem xét trường hợp có một khoảng duy nhất bao gồm các vị trí từ 1 đến 3. Khoảng này có độ dài lẻ. Trong tình huống như vậy, bất kỳ nỗ lực nào nhằm cân bằng các ký hiệu cục bộ bên trong khoảng sẽ ngay lập tức dẫn đến tình trạng không khớp chẵn lẻ, nhưng một người kiểm tra ngây thơ vẫn có thể cố gắng gán một cách tham lam và không nhận ra rằng sự thay thế đã đáp ứng mọi thứ trên toàn cầu. 

Mặt khác, nếu tất cả các khoảng có độ dài chẵn, người ta có thể giả định tính khả thi một cách không chính xác, nhưng logic dự định cho thấy rằng sự cân bằng chẵn lẻ trở nên quá cứng nhắc trên toàn cầu và không có phép gán nhất quán nào có thể đáp ứng tất cả các ràng buộc trừ khi mâu thuẫn được gây ra bởi ít nhất một khoảng có độ dài lẻ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng gán cho mỗi vị trí một trong hai ký hiệu và xác minh tất cả các ràng buộc về khoảng thời gian. Vì mỗi vị trí có hai lựa chọn, điều này dẫn đến$2^n$các nhiệm vụ có thể. Đối với mỗi nhiệm vụ, kiểm tra tất cả$m$khoảng thời gian mất$O(m)$, mang lại tổng cộng$O(m \cdot 2^n)$, vượt xa mọi giới hạn khả thi khi$n$là lớn. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng toàn bộ cấu trúc được điều chỉnh bởi tính chẵn lẻ. Các ràng buộc không phụ thuộc vào sự sắp xếp chính xác của các ký hiệu mà phụ thuộc vào việc liệu các khoảng có tạo ra sự mất cân bằng giữa các vị trí được lập chỉ mục chẵn và lẻ hay không. Bất kỳ khoảng có độ dài lẻ nào đều tạo ra sự mất cân bằng như vậy, trong khi các khoảng có độ dài chẵn bảo toàn tính đối xứng chẵn lẻ. 

Điều này dẫn đến một đặc tính trực tiếp đáng ngạc nhiên: lần duy nhất một kết cấu hợp lệ bị ép buộc là khi tồn tại ít nhất một khoảng có độ dài lẻ. Trong trường hợp đó, một mẫu xen kẽ đơn giản như MTMTMT... đã đáp ứng tất cả các ràng buộc vì mọi cặp liền kề đều cân bằng và mọi khoảng đều chứa cấu trúc chẵn lẻ bằng nhau hoặc bù. 

Vì vậy, thay vì xây dựng bất cứ thứ gì, chúng ta chỉ cần quét các khoảng và kiểm tra độ dài của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc số vị trí và số khoảng. Cấu trúc của bài toán cho phép chúng ta xử lý từng trường hợp thử nghiệm như một hệ thống chẵn lẻ mới. 
2. Duy trì một cờ boolean để theo dõi xem chúng ta có gặp bất kỳ khoảng nào có độ dài lẻ hay không. Cờ này thể hiện sự tồn tại của ràng buộc phá vỡ tính chẵn lẻ. 
3. Đối với mỗi khoảng thời gian$[l, r]$, tính độ dài của nó là$r - l + 1$. Nếu giá trị này là số lẻ, hãy đặt cờ thành true. Lý do điều này là đủ là vì bất kỳ đoạn có độ dài lẻ nào cũng gây ra sự mất cân bằng không thể tránh khỏi giữa các vị trí chẵn lẻ xen kẽ. 
4. Sau khi xử lý tất cả các khoảng thời gian, hãy quyết định câu trả lời chỉ dựa vào lá cờ. Nếu tồn tại ít nhất một khoảng độ dài lẻ, hãy đưa ra kết quả là có thể xây dựng hợp lệ. Nếu không thì xuất ra điều đó là không thể. 

### Tại sao nó hoạt động 

Không gian xây dựng được điều chỉnh hoàn toàn bởi tính nhất quán chẵn lẻ. Nhiệm vụ luân phiên MTMTMT... cân bằng hoàn hảo sự đóng góp từ các vị trí chẵn và lẻ trên bất kỳ phân khúc nào. Nếu một khoảng có độ dài lẻ, nó buộc hệ thống vào trạng thái trong đó sự luân phiên đó không phải là tùy chọn mà được thực thi về mặt cấu trúc ở đâu đó trong ví dụ, đảm bảo tính khả thi. Nếu mọi khoảng đều có độ dài chẵn thì không tồn tại cơ chế ép buộc như vậy và yêu cầu tổng thể về số lượng bằng nhau không thể được điều hòa đồng thời với tất cả các ràng buộc, khiến hệ thống không nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        has_odd = False
        
        for _ in range(m):
            l, r = map(int, input().split())
            if (r - l + 1) % 2 == 1:
                has_odd = True
        
        if has_odd:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp hoàn toàn là kiểm tra luồng qua đầu vào. Trạng thái duy nhất chúng tôi giữ là liệu có tồn tại khoảng thời gian lẻ hay không. Điều này tránh mọi nỗ lực xây dựng trình tự thực tế, điều này là không cần thiết vì tính khả thi được xác định về mặt cấu trúc. 

Một lỗi triển khai phổ biến là thoát sớm một trường hợp thử nghiệm và không sử dụng tất cả các dòng đầu vào còn lại. Vì sự cố là thử nghiệm nhiều lần nên việc không đọc tất cả các khoảng thời gian cho một trường hợp thử nghiệm sẽ không đồng bộ hóa luồng đầu vào và làm hỏng tất cả các trường hợp tiếp theo. Vòng lặp phải luôn xử lý đầy đủ số khoảng đã khai báo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp thử nghiệm với các khoảng$[1, 2]$Và$[3, 5]$. 

| Khoảng thời gian | Chiều dài | Số lẻ? | has_odd | 
| --- | --- | --- | --- | 
| 1-2 | 2 | Không | Sai | 
| 3-5 | 3 | Có | Đúng | 

Sau khi xử lý, cờ trở thành true nên kết quả đầu ra là CÓ. 

Điều này chứng tỏ một khoảng lẻ duy nhất chi phối quyết định như thế nào bất kể các ràng buộc khác. 

### Ví dụ 2 

Xem xét khoảng thời gian$[1, 2]$,$[2, 3]$,$[4, 5]$. 

| Khoảng thời gian | Chiều dài | Số lẻ? | has_odd | 
| --- | --- | --- | --- | 
| 1-2 | 2 | Không | Sai | 
| 2-3 | 2 | Không | Sai | 
| 4-5 | 2 | Không | Sai | 

Không có khoảng thời gian nào đưa ra sự mất cân bằng chẵn lẻ, vì vậy kết quả là KHÔNG. 

Điều này cho thấy trường hợp bổ sung trong đó cấu trúc quá đồng nhất để cho phép xây dựng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi khoảng được kiểm tra một lần và đầu vào được xử lý tuyến tính | 
| Không gian |$O(1)$| Chỉ có một cờ boolean duy nhất được duy trì | 

Các ràng buộc dễ dàng được thỏa mãn vì giải pháp chỉ thực hiện công việc không đổi trong mỗi khoảng thời gian và tránh mọi hình thức xây dựng hoặc mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            has_odd = False
            for _ in range(m):
                l, r = map(int, input().split())
                if (r - l + 1) % 2 == 1:
                    has_odd = True
            output.append("YES" if has_odd else "NO")
    
    solve()
    return "\n".join(output)

# sample-like tests
assert run("1\n5 2\n1 2\n3 5") == "YES"
assert run("1\n5 3\n1 2\n2 3\n4 5") == "NO"

# custom tests
assert run("1\n1 1\n1 1") == "YES", "single odd interval"
assert run("1\n2 1\n1 2") == "NO", "only even interval"
assert run("2\n3 1\n1 3\n4 2\n1 2\n2 4") == "YES\nNO", "multi test mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng lẻ duy nhất | CÓ | trường hợp kích hoạt tối thiểu | 
| chỉ khoảng chẵn | KHÔNG | trường hợp chẵn lẻ thống nhất | 
| hỗn hợp thử nghiệm đa | CÓ / KHÔNG | tính chính xác của việc tách đầu vào | 

## Vỏ cạnh 

Trường hợp một cạnh là một khoảng duy nhất bao phủ toàn bộ phạm vi có độ dài 1. Thuật toán ngay lập tức đánh dấu nó là số lẻ và trả về CÓ. Hành vi đúng xảy ra vì một vị trí đã phá vỡ tính đối xứng chẵn lẻ và cờ nắm bắt chính xác điều này mà không cần bất kỳ cấu trúc nào. 

Một trường hợp khác là khi tất cả các khoảng đều có độ dài 2, có khả năng chồng chéo. Đối với đầu vào như$[1,2], [2,3], [3,4]$, mọi khoảng đều có độ dài chẵn, do đó cờ vẫn sai trong suốt quá trình xử lý và câu trả lời là KHÔNG. Thuật toán xử lý chính xác các ràng buộc chồng chéo vì sự chồng chéo không ảnh hưởng đến việc phân loại chẵn lẻ. 

Trường hợp tinh tế cuối cùng là đầu vào lớn trong đó số khoảng là tối đa. Vì giải pháp không lưu trữ các khoảng thời gian hoặc cố gắng xây dựng lại nên nó xử lý từng khoảng thời gian một cách an toàn và tránh các vấn đề về áp lực bộ nhớ hoặc ngăn xếp.
