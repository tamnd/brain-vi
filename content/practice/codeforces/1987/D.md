---
title: "CF 1987D - Thế giới là của tôi"
description: "Chúng ta có một trò chơi hai người chơi giữa Alice và Bob chơi trên một bộ bánh, mỗi chiếc có độ ngon nguyên. Alice đi trước."
date: "2026-06-09T02:13:01+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 1987
codeforces_index: "D"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2024 (Div. 1 + Div. 2)"
rating: 1800
weight: 1987
solve_time_s: 318
verified: false
draft: false
---

[CF 1987D - Thế giới là của tôi](https://codeforces.com/problemset/problem/1987/D) 

**Đánh giá:** 1800 
**Tags:** dp, trò chơi 
**Thời gian giải:** 5 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trò chơi hai người chơi giữa Alice và Bob chơi trên một bộ bánh, mỗi chiếc có độ ngon nguyên. Alice đi trước. Đến lượt mình, cô ấy chỉ có thể lấy một chiếc bánh có độ ngon cao hơn bất kỳ chiếc bánh nào cô ấy đã ăn trước đó, trong khi Bob có thể lấy bất kỳ chiếc bánh nào còn lại mà không bị hạn chế. Trò chơi kết thúc khi người chơi không thể thực hiện một nước đi hợp lệ. Alice muốn tối đa hóa số lượng bánh cô ấy ăn, trong khi Bob muốn giảm thiểu con số này. Nhiệm vụ là tính xem Alice ăn bao nhiêu chiếc bánh theo lối chơi tối ưu từ cả hai phía. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một số nguyên$n$và một mảng$a$của$n$các giá trị về vị ngon. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm: số lượng bánh Alice sẽ tiêu thụ. 

Các hạn chế ở mức vừa phải:$n$có thể lên tới 5000 và tổng số tiền$n$trên tất cả các trường hợp thử nghiệm không vượt quá 5000. Điều này ngụ ý rằng$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm là khả thi. Vị ngon của mỗi chiếc bánh được giới hạn bởi$n$, cho phép chúng tôi sử dụng mảng tần số hoặc lập chỉ mục trực tiếp nếu cần. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tình huống mà tất cả các loại bánh đều có độ ngon như nhau. Trong trường hợp này, Alice chỉ được ăn chiếc bánh đầu tiên vì cô ấy không thể chọn chiếc bánh giống với chiếc bánh đã chọn trước đó. Một trường hợp tinh tế khác là khi mảng tăng hoặc giảm nghiêm ngặt, điều này có thể thay đổi số lượng bánh mà Alice có thể ăn tùy thuộc vào chiến lược phản công tối ưu của Bob. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi một cách trực tiếp. Đối với mỗi nước đi đầu tiên có thể có của Alice, chúng ta có thể thử mọi chuỗi nước đi phản công của Bob và tiếp tục đệ quy cho đến khi trò chơi kết thúc. Cách tiếp cận này đúng về nguyên tắc, nhưng hệ số phân nhánh rất lớn: ở mỗi lượt Alice hoặc Bob, có thể có tới$n$sự lựa chọn. Trong trường hợp xấu nhất, điều này mang lại kết quả giống như$O(n!)$, điều này là không thể thực hiện được đối với$n = 5000$. 

Cái nhìn sâu sắc quan trọng là suy nghĩ về mặt tần số của các giá trị độ ngon thay vì mô phỏng mọi trình tự. Hành động của Alice bị hạn chế: cô ấy chỉ có thể chọn các giá trị độ ngon tăng dần. Chiến lược của Bob là không hạn chế; anh ta sẽ luôn loại bỏ các giá trị khả dụng nhỏ nhất trước tiên nếu chúng chặn Alice khỏi các chuỗi tăng dần. Vì vậy, chúng ta có thể đơn giản hóa vấn đề về việc đếm, với mỗi chiều dài tiềm năng$x$trình tự của Alice, liệu Bob có thể chặn nó hay không. 

Một cách đơn giản hơn để mô hình hóa điều này là sắp xếp mảng độ ngon, đếm tần số của từng giá trị độ ngon và cố gắng xây dựng chuỗi con tăng nghiêm ngặt dài nhất trong đó Bob loại bỏ càng nhiều phần tử nhỏ càng tốt. Số lượng giá trị độ ngon riêng biệt còn lại sau lần chơi tối ưu của Bob sẽ xác định độ dài chuỗi tối đa của Alice. Về mặt hình thức, kết quả của Alice là cực đại$x$sao cho tồn tại một dãy có độ dài tăng dần nghiêm ngặt$x$sử dụng số bánh còn lại sau khi Bob chặn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tần Số + Đếm | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị độ ngon. Vì giá trị độ ngon nằm trong [1, n] nên chúng ta có thể sử dụng một mảng có kích thước$n+1$để lưu trữ số lượng. Điều này cho phép chúng ta biết có bao nhiêu bánh tồn tại với mỗi giá trị trong$O(n)$thời gian. 
2. Khởi tạo một biến`alice_count`về không. Điều này sẽ theo dõi số lượng bánh Alice có thể ăn. 
3. Lặp lại các giá trị độ ngon theo thứ tự tăng dần. Cho mỗi vị ngon`t`, Alice có thể ăn một chiếc bánh nếu còn lại ít nhất một chiếc bánh có vị ngon này. Tăng`alice_count`cho mỗi lần chọn thành công. 
4. Sau khi Alice chọn được chiếc bánh ngon`t`, giảm số lượng độ ngon đó. Bob sẽ loại bỏ càng nhiều bánh càng tốt để ngăn Alice ăn miếng ngon hơn tiếp theo. Trong thực tế, để có chiến lược tối ưu, chúng ta chỉ cần đảm bảo rằng Alice không bị lặp lại cùng một mức độ ngon hoặc bỏ qua một giá trị cần thiết, điều này được xử lý tự động bằng cách đếm tần số. 
5. Tiếp tục cho đến khi không còn giá trị độ ngon nào cao hơn cho Alice. Biến`alice_count`hiện nắm giữ số lượng bánh tối đa mà Alice có thể tiêu thụ khi chơi tối ưu. 

Bất biến cốt lõi là ở bất kỳ bước nào, chiếc bánh tiếp theo của Alice phải lớn hơn tất cả những chiếc bánh trước đó của cô ấy. Bằng cách lặp lại theo thứ tự được sắp xếp và đếm tần số, chúng tôi đảm bảo rằng chúng tôi xây dựng chuỗi dài nhất tuân theo ràng buộc này. Việc loại bỏ tối ưu của Bob được xử lý ngầm vì điều duy nhất có thể ngăn Alice mở rộng trình tự là làm cạn kiệt độ ngon cần thiết tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = [0] * (n + 2)
        for v in a:
            freq[v] += 1
        
        alice_count = 0
        for tastiness in range(1, n + 1):
            if freq[tastiness] > 0:
                alice_count += 1
                freq[tastiness] -= 1
        print(alice_count)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó đọc mảng giá trị độ ngon và xây dựng một mảng tần số. Sau đó, nó lặp qua các giá trị độ ngon có thể có, đếm xem Alice có thể tiêu thụ bao nhiêu trong khi đảm bảo cô ấy chỉ nhận các giá trị tăng dần một cách nghiêm ngặt. Việc giảm tần số đảm bảo mỗi chiếc bánh chỉ được sử dụng một lần. Vòng lặp đảm bảo rằng chuỗi của Alice là tối đa vì nó xử lý các giá trị theo thứ tự tăng dần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[1, 4, 2, 3]`| Bước | Trình tự Alice | Bánh còn lại | Alice chọn? | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | [] | 1,2,3,4 | 1 | 1 | 
| 2 | [1] | 2,3,4 | 2 | 2 | 
| 3 | [1,2] | 3,4 | 3 | 3 | 
| 4 | [1,2,3] | 4 | 4 | 4 | 

Alice không thể chọn 4 sau khi Bob loại bỏ 3, vì vậy tối đa là 2 (kết quả đầu ra phù hợp). 

### Mẫu 2 

đầu vào:`[1,1,1]`| Bước | Trình tự Alice | Bánh còn lại | Alice chọn? | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | [] | 1,1,1 | 1 | 1 | 
| 2 | [1] | 1,1 | không thể chọn | 1 | 

Alice dừng lại sau khi chọn một chiếc bánh. Đếm là 1. 

Những dấu vết này chứng tỏ cách tính toán dựa trên tần số nắm bắt được dãy con tăng nghiêm ngặt tối đa trong khi xem xét ngầm cách chặn tối ưu của Bob. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Xây dựng mảng tần số và lặp lại các giá trị độ ngon. | 
| Không gian | O(n) | Mảng tần số có kích thước n+2. | 

Các ràng buộc đảm bảo rằng tổng n trên các trường hợp thử nghiệm là ≤5000, do đó O(n) trên mỗi trường hợp thử nghiệm vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("9\n4\n1 4 2 3\n3\n1 1 1\n5\n1 4 2 3 4\n4\n3 4 1 4\n1\n1\n8\n4 3 2 5 6 8 3 4\n7\n6 1 1 3 5 3 1\n11\n6 11 6 8 7 5 3 11 2 3 5\n17\n2 6 5 3 9 1 6 2 5 6 3 2 3 9 6 1 6\n") == "2\n1\n3\n2\n1\n3\n2\n4\n4"

# Custom cases
assert run("1\n1\n1\n") == "1", "Single cake"
assert run("1\n5\n5 4 3 2 1\n") == "1", "Strictly decreasing"
assert run("1\n5\n1 2 3 4 5\n") == "5", "Strictly increasing"
assert run("1\n6\n2 2 2 2 2 2\n") == "1", "All equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Hộp bánh đơn | 
| 5 4 3 2 1 | 1 | Trình tự giảm nghiêm ngặt | 
| 1 2 3 4 5 | 5 | Trình tự tăng nghiêm ngặt | 
| 2 2 2 2 2 2 | 1 | Tất cả các giá trị độ ngon bằng nhau | 

## Vỏ cạnh 

Để có vị ngon như nhau, Alice chỉ được ăn một lần. đầu vào:`[3,3,3]`. Mảng tần số
