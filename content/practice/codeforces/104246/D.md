---
title: "CF 104246D - Phân phối Pizza"
description: "Một chiếc bánh pizza được cắt thành những lát giống hệt nhau và mỗi lát là một hình cung được xác định bởi một góc ở tâm cố định θ. Bán kính r không liên quan đến câu hỏi phân phối vì tất cả các lát cắt đều bằng nhau; điều quan trọng chỉ là có bao nhiêu phần góc bằng nhau trong toàn bộ vòng tròn 360° được phân chia…"
date: "2026-07-01T23:01:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "D"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 83
verified: false
draft: false
---

[CF 104246D - Phân phối Pizza](https://codeforces.com/problemset/problem/104246/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Một chiếc bánh pizza được cắt thành những lát giống hệt nhau và mỗi lát là một hình cung được xác định bởi một góc ở tâm cố định θ. Bán kính r không liên quan đến câu hỏi phân phối vì tất cả các lát cắt đều bằng nhau; điều quan trọng chỉ là có bao nhiêu phần góc bằng nhau mà toàn bộ vòng tròn 360° được chia thành. 

Từ tuyên bố, chúng tôi không được cung cấp trực tiếp số lát. Thay vào đó, chúng ta được cho biết góc lát cắt θ, góc này xác định ngầm số lượng lát cắt là 360 / θ và chúng ta được đảm bảo đây là số nguyên cho mọi trường hợp thử nghiệm. 

Hai người muốn chia chiếc bánh pizza sao cho cả hai đều nhận được số nguyên lát và cả hai đều nhận được số lát bằng nhau. Vì các lát cắt không thể chia nhỏ được nên vấn đề giảm xuống còn việc kiểm tra xem tổng số lát cắt có phải là số chẵn hay không. 

Kích thước đầu vào nhỏ, lên tới 2400 trường hợp thử nghiệm và số học theo thời gian không đổi cho mỗi trường hợp. Điều này gợi ý rõ ràng rằng bất kỳ giải pháp O(1) nào cho mỗi trường hợp thử nghiệm đều là đủ và ngay cả việc kiểm tra số học hoặc mô-đun đơn giản cũng nằm trong giới hạn. Bất cứ điều gì liên quan đến việc lặp lại các lát cắt hoặc phân tích nhân tố đều không cần thiết. 

Một trường hợp cạnh tinh tế phát sinh khi θ lớn, chẳng hạn như θ = 360. Trong trường hợp đó, chỉ có một lát cắt và không thể chia đều cho hai người. Một trường hợp góc khác là θ = 180, trong đó có chính xác hai lát cắt và có thể phân chia công bằng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là trước tiên hãy tính số lát cắt n = 360 / θ. Khi chúng ta có n, câu hỏi đặt ra là liệu n có thể chia thành hai số nguyên bằng nhau hay không, nghĩa là n có chia hết cho 2 hay không. 

Tư duy vũ phu sẽ là mô phỏng việc cắt bánh pizza hoặc đếm các lát một cách rõ ràng, nhưng điều đó đã không cần thiết vì số lượng lát được xác định bằng một phép chia đơn giản. Bất kỳ cách tiếp cận nào lặp lại xung quanh vòng tròn hoặc xây dựng các lát cắt một cách rõ ràng sẽ vẫn chạy trong thời gian không đổi cho mỗi trường hợp nhưng về mặt khái niệm là quá mức cần thiết. 

Quan sát quan trọng là sự công bằng giữa hai người tương đương với việc hỏi liệu tổng số đơn vị giống hệt nhau có phải là số chẵn hay không. Hình học và bán kính không có vai trò gì ngoài việc đảm bảo rằng các lát cắt bằng nhau; chỉ có tính chẵn lẻ của số lát là quan trọng. 

Do đó, vấn đề quy về việc kiểm tra xem (360 / θ) % 2 == 0 hay không, có thể viết lại thành kiểm tra xem 360 / θ có chẵn hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm lát trực tiếp | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra chẵn lẻ tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc θ cho mỗi trường hợp thử nghiệm và tính số lát n bằng 360 chia cho θ. Bài toán đảm bảo tính chia hết nên không xảy ra vấn đề làm tròn. 
2. Kiểm tra xem n có chia hết cho 2 hay không. Điều này xác định xem các lát cắt có thể được chia thành hai nhóm bằng nhau mà không làm vỡ bất kỳ lát cắt nào hay không. 
3. Nếu n chẵn, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

### Tại sao nó hoạt động 

Mỗi lát là một đơn vị tài nguyên không thể chia cắt. Bất kỳ phân phối hợp lệ nào cũng chỉ gán toàn bộ lát cắt, do đó vấn đề giảm xuống việc phân chia n đối tượng giống hệt nhau thành hai bộ có kích thước bằng nhau. Một phân vùng như vậy tồn tại khi và chỉ khi n chẵn. Không có thuộc tính hình học nào ngoài tổng số lượng ảnh hưởng đến tính khả thi, vì vậy tính chẵn lẻ vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        r, theta = map(int, input().split())
        n = 360 // theta
        if n % 2 == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và bỏ qua r, vì nó không ảnh hưởng đến số lượng lát cắt. Phép chia số nguyên 360 // θ là an toàn vì bài toán đảm bảo rằng θ chia hết cho 360. Việc kiểm tra tính chẵn lẻ trực tiếp mã hóa điều kiện khả thi. 

Một lỗi triển khai phổ biến là cố gắng chia dấu phẩy động cho 360 / θ và sau đó kiểm tra tính chẵn lẻ, điều này có thể gây ra các vấn đề về độ chính xác trong các vấn đề khác. Ở đây, việc sử dụng phép chia số nguyên sẽ tránh được mọi rủi ro như vậy và phù hợp với cấu trúc riêng biệt của các lát cắt. 

## Ví dụ đã hoạt động 

Hãy xem xét các giá trị θ tạo ra số lượng lát cắt khác nhau. 

### Ví dụ 1 

đầu vào:```
r = 10, θ = 45
```Ở đây n = 360/45 = 8. 

| Bước | Giá trị | 
| --- | --- | 
| θ | 45 | 
| n = 360/θ | 8 | 
| n % 2 | 0 | 
| Đầu ra | CÓ | 

Vì 8 lát có thể được chia thành 4 và 4 nên kết quả là CÓ. 

### Ví dụ 2 

đầu vào:```
r = 5, θ = 60
```Ở đây n = 360/60 = 6. 

| Bước | Giá trị | 
| --- | --- | 
| θ | 60 | 
| n = 6 | | 
| n % 2 | 0 | 
| Đầu ra | CÓ | 

Điều này cho thấy số lượng lát chẵn vẫn mang lại sự phân chia hợp lệ. 

### Ví dụ 3 

đầu vào:```
r = 8, θ = 120
```Ở đây n = 360/120 = 3. 

| Bước | Giá trị | 
| --- | --- | 
| θ | 120 | 
| n = 3 | | 
| n % 2 | 1 | 
| Đầu ra | KHÔNG | 

Điều này chứng tỏ trường hợp thất bại khi các lát cắt có số lượng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu số học theo thời gian không đổi và kiểm tra modulo | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài một vài biến | 

Các ràng buộc cho phép tối đa 2400 trường hợp thử nghiệm, do đó việc quét tuyến tính trên đầu vào là không đáng kể trong giới hạn thời gian. Mỗi lần lặp chỉ thực hiện một vài thao tác số nguyên, đảm bảo giải pháp chạy thoải mái trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        r, theta = map(int, input().split())
        n = 360 // theta
        out.append("YES" if n % 2 == 0 else "NO")

    return "\n".join(out)

# provided samples
assert run("3\n10 45\n5 40\n8 60\n") == "YES\nNO\nYES"

# custom cases
assert run("1\n1 360\n") == "NO", "single slice"
assert run("1\n1 180\n") == "YES", "two slices"
assert run("1\n10 90\n") == "YES", "four slices"
assert run("1\n10 120\n") == "NO", "three slices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 360 | KHÔNG | lát đơn không thể chia được | 
| 1 180 | CÓ | chia chẵn hợp lệ tối thiểu | 
| 1 90 | CÓ | trường hợp chẵn điển hình | 
| 1 120 | KHÔNG | trường hợp số lát lẻ | 

## Vỏ cạnh 

Khi θ = 360, chiếc bánh pizza chỉ gồm một lát. Thuật toán tính toán n = 1, phát hiện tính chẵn lẻ lẻ và trả về chính xác NO. 

Khi θ = 180, có đúng hai lát cắt. Phép tính cho kết quả n = 2 và tính chẵn lẻ là chẵn, tạo ra CÓ, khớp với mức phân chia công bằng duy nhất có thể có. 

Khi θ = 1 hoặc bất kỳ ước số nhỏ nào của 360, thuật toán vẫn hoạt động nhất quán vì nó chỉ dựa vào số học số nguyên và tính chẵn lẻ, do đó không phát sinh vấn đề về độ chính xác hoặc tỷ lệ.
