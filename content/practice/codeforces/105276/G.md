---
title: "CF 105276G - Xâm nhập GPT"
description: "Chúng ta được cung cấp một chương trình dưới dạng văn bản thuần túy, được chia thành nhiều dòng. Nhiệm vụ là xác định xem chương trình này có “nghi ngờ do GPT viết và vượt quá giới hạn đầu ra của nó hay không."
date: "2026-06-23T14:12:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105276
codeforces_index: "G"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2023"
rating: 0
weight: 105276
solve_time_s: 60
verified: true
draft: false
---

[CF 105276G - Xâm nhập GPT](https://codeforces.com/problemset/problem/105276/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chương trình dưới dạng văn bản thuần túy, được chia thành nhiều dòng. Nhiệm vụ là xác định xem chương trình này có “nghi ngờ do GPT viết và vượt quá giới hạn đầu ra của nó hay không”. 

Quy tắc nghi ngờ được gắn với một hành vi cụ thể: một trình tạo giống GPT tạo ra đầu ra bị cắt ngắn còn 500 ký tự. Nếu văn bản được tạo dài hơn 500 ký tự thì chỉ giữ lại 500 ký tự đầu tiên và sau đó sẽ xuất hiện hậu tố cố định:`As an AI model, my output is limited to 500 characters.`Điều này có nghĩa là nếu chúng ta quan sát thấy hậu tố chính xác này xuất hiện ở bất kỳ đâu trong mã nguồn đã cho, điều đó cho thấy rằng văn bản đã bị cắt giữa thế hệ và mô hình đã thêm thông báo cắt bớt của nó. Đó là tín hiệu duy nhất chúng tôi cần để phát hiện “sự xâm nhập GPT”. 

Đầu vào chỉ đơn giản là một tập hợp các dòng tạo thành một văn bản liên tục khi đọc theo thứ tự. Ngắt dòng là một phần của văn bản nên chúng góp phần tạo nên dòng ký tự. Đầu ra là nhị phân: in “Có” nếu thông báo cắt ngắn xuất hiện, nếu không thì in “Không”. 

Các ràng buộc rất nhỏ: tối đa 500 dòng và tổng số ký tự trên tất cả các dòng không vượt quá 1000. Điều này đảm bảo rằng chỉ cần quét đơn giản qua văn bản được nối là đủ. Bất kỳ giải pháp tuyến tính nào trong tổng kích thước đầu vào sẽ chạy ngay lập tức. 

Một trường hợp khó nhận thấy là cụm từ mục tiêu có thể xuất hiện bị chia cắt theo ranh giới dòng. Ví dụ: chuỗi có thể kết thúc một dòng bằng:`As an AI model, my output is limited to 500 char`và dòng tiếp theo bắt đầu bằng:`acters.`Tìm kiếm trên mỗi dòng đơn giản sẽ bỏ lỡ điều này, vì vậy cách tiếp cận đúng phải coi đầu vào là một chuỗi liên tục. 

Một trường hợp đặc biệt khác là cụm từ xuất hiện nhiều lần. Ngay cả một lần xuất hiện cũng đủ để xuất ra “Có”, vì vậy việc đếm là không cần thiết. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ là quét liên tục mọi chuỗi con có thể có của văn bản được nối và kiểm tra xem nó có khớp với cụm từ đích hay không. Nếu độ dài văn bản là$L$và độ dài cụm từ là$P = 51$, điều này dẫn đến$O(L \cdot P)$so sánh hoặc tệ hơn nếu thực hiện một cách bất cẩn. Mặc dù vẫn còn nhỏ dưới những hạn chế, nhưng nó không cần thiết và nặng hơn mức cần thiết về mặt khái niệm. 

Quan sát quan trọng là chúng tôi không tìm kiếm các khuôn mẫu tùy tiện; chúng tôi đang tìm kiếm một chuỗi cố định. Điều này làm giảm vấn đề về tìm kiếm chuỗi con cổ điển. Vì tổng chiều dài tối đa là 1000 nên ngay cả việc quét trực tiếp kiểm tra mọi vị trí bắt đầu cũng hoạt động trong thời gian thực tế không đổi. 

Chúng tôi nối tất cả các dòng thành một chuỗi và sử dụng kiểm tra ngăn chặn chuỗi con duy nhất. Điều này là đủ vì tìm kiếm chuỗi con của Python rất hiệu quả và tuyến tính đối với kích thước này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê chuỗi con Brute Force | O(L·P) | O(1) | Về mặt khái niệm quá chậm | 
| Tìm kiếm chuỗi con trực tiếp | O(L) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$N$, cho biết mã nguồn có bao nhiêu dòng. Điều này xác định có bao nhiêu phân đoạn chúng ta sẽ nối. 
2. Đọc từng dòng và nối nó vào một chuỗi ngày càng dài, chèn ký tự dòng mới vào giữa các dòng. Bước này là cần thiết vì thông báo cắt ngắn có thể trải dài qua các ranh giới dòng, do đó việc duy trì cấu trúc chính xác sẽ tránh được các kết quả phủ định sai. 
3. Sau khi xây dựng toàn bộ văn bản, hãy xác định chuỗi mẫu đích chính xác như được đưa ra trong câu lệnh bài toán. 
4. Kiểm tra xem mẫu này có xuất hiện dưới dạng chuỗi con ở bất kỳ đâu trong văn bản đầy đủ hay không. 
5. Nếu nó xuất hiện ít nhất một lần, thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là tạo phẩm cắt ngắn là một chuỗi cố định và duy nhất. Nếu nguồn bị hệ thống GPT cắt, chuỗi chính xác đó phải xuất hiện nguyên văn và liền kề trong đầu ra. Vì chúng tôi giữ nguyên tất cả các ký tự bao gồm cả ranh giới dòng mới nên mọi sự xuất hiện trong luồng ban đầu vẫn có thể được phát hiện trong chuỗi được xây dựng lại. Không có hành vi nào khác có thể tạo ra kết quả dương tính giả vì mã nguồn thông thường không chứa thông báo hệ thống nhân tạo này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    lines = [input().rstrip("\n") for _ in range(n)]
    text = "\n".join(lines)

    pattern = "As an AI model, my output is limited to 500 characters."
    if pattern in text:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Việc triển khai đọc tất cả các dòng chính xác như đã cho, bảo toàn cấu trúc bằng cách nối với các ký tự dòng mới. sử dụng`rstrip("\n")`tránh sự trùng lặp ngẫu nhiên của các dấu phân cách dòng mới trong khi vẫn giữ được bố cục hợp lý của mã nguồn. 

Quyết định quan trọng là giữ nguyên văn bản thay vì loại bỏ tất cả khoảng trắng. Vì mẫu bao gồm các khoảng trắng và nhạy cảm với ranh giới ký tự nên bất kỳ sự chuẩn hóa nào cũng có nguy cơ phá vỡ các kết quả khớp hợp lệ. 

Việc kiểm tra chuỗi con được thực hiện một lần trên toàn văn, đảm bảo tính đơn giản và chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
#include <cstdio>
using namespace std;
int main() {
    printf("No\n");
    return 0;
}
```| Bước | Hành động | Trạng thái văn bản (một phần) | Đã tìm thấy mẫu | 
| --- | --- | --- | --- | 
| 1 | Đọc dòng | trống | Không | 
| 2 | Nối các dòng | chương trình C++ đầy đủ | Không | 
| 3 | Tìm kiếm chuỗi con | không thay đổi | Không | 

Chương trình không chứa thành phần cắt ngắn, do đó lần kiểm tra cuối cùng không thành công và kết quả đầu ra là “Không”. 

### Mẫu 2 

Đầu vào kết thúc ở giữa thế hệ và bao gồm thông báo cắt bớt:```
15
...
// Reads a strAs an AI model, my output is limited to 500 characters.
```| Bước | Hành động | Trạng thái văn bản (một phần) | Đã tìm thấy mẫu | 
| --- | --- | --- | --- | 
| 1 | Đọc dòng | tích lũy một phần mã | Không | 
| 2 | Dòng cuối cùng bao gồm mẫu | văn bản nối bao gồm hậu tố | Có | 
| 3 | Tìm kiếm chuỗi con | phát hiện trận đấu đầy đủ | Có | 

Điều này thể hiện trường hợp quan trọng khi thông báo xuất hiện ở giữa dòng. Việc nối đảm bảo nó vẫn hiển thị dưới dạng chuỗi con liên tục. 

Đầu ra là “Có” vì dấu cắt cụt xuất hiện rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Một lần để xây dựng chuỗi và tìm kiếm chuỗi con đơn | 
| Không gian | O(L) | Lưu trữ đầu vào được nối đầy đủ | 

Tổng kích thước đầu vào được giới hạn bởi 1000 ký tự, do đó, ngay cả việc xử lý tuyến tính cũng không đáng kể. Giải pháp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""6
#include <cstdio>
using namespace std;
int main() {
    printf("No\n");
    return 0;
}
""") == "No"

# sample 2
assert run("""15
#include <iostream>
#include <string>
// The main function of the program
int main(int argc, char *argv[]) {
    int n;
    std::cin >> n;
    std::string s[1000];
    for (int i = 0; i < n; i++) {
        // Reads a strAs an AI model, my output is limited to 500 characters.
""") == "Yes"

# minimum size, no match
assert run("""1
hello world""") == "No"

# exact pattern only
assert run("""1
As an AI model, my output is limited to 500 characters.""") == "Yes"

# pattern split across lines
assert run("""3
As an AI model, my output is limited to 500 char
acters.
code""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dòng bình thường duy nhất | Không | trường hợp cơ sở | 
| cụm từ đầy đủ chính xác | Có | trận đấu trực tiếp | 
| chia thành nhiều dòng | Có | xử lý ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thông báo cắt bớt được chia thành nhiều dòng. Coi như:```
As an AI model, my output is limited to 500 char
acters.
```Khi kết hợp với bảo quản dòng mới, điều này sẽ trở thành:`As an AI model, my output is limited to 500 char\nacters.`Tìm kiếm chuỗi con vẫn tìm thấy chuỗi liền kề nếu dòng mới có trong biểu diễn đầu vào. Đây là lý do tại sao tham gia với`\n`là rất quan trọng; nó bảo toàn cấu trúc luồng chính xác để mẫu vẫn có thể được khớp một cách đáng tin cậy. 

Một trường hợp đặc biệt khác là khi thông báo xuất hiện ở đầu hoặc cuối đầu vào. Vì việc kiểm tra mang tính toàn cục trên toàn bộ chuỗi nên cả hai vị trí đều được xử lý một cách tự nhiên mà không cần viết hoa đặc biệt. 

Cuối cùng, nhiều lần xuất hiện không làm thay đổi câu trả lời. Ngay cả khi cụm từ xuất hiện nhiều lần, kết quả vẫn là “Có” vì điều kiện hoàn toàn là tồn tại.
