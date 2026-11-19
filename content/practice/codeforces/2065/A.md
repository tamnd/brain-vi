---
title: "CF 2065A - Skibidus và Amog'u"
description: "Chúng ta được cung cấp một từ đã ở dạng số ít trong một ngôn ngữ đồ chơi rất hạn chế. Mỗi từ được tạo từ một chuỗi gốc và dạng số ít luôn được tạo bằng cách thêm hậu tố “us” vào gốc đó."
date: "2026-06-08T07:20:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2065
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1003 (Div. 4)"
rating: 800
weight: 2065
solve_time_s: 276
verified: false
draft: false
---

[CF 2065A - Skibidus và Amog'u](https://codeforces.com/problemset/problem/2065/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, triển khai, chuỗi 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ đã ở dạng số ít trong một ngôn ngữ đồ chơi rất hạn chế. Mỗi từ được tạo từ một chuỗi gốc và dạng số ít luôn được tạo bằng cách thêm hậu tố “us” vào gốc đó. Nhiệm vụ của chúng ta là đảo ngược quá trình này: loại bỏ hậu tố số ít và chuyển gốc còn lại thành dạng số nhiều bằng cách thêm “i”. 

Kích thước đầu vào cực kỳ nhỏ, mỗi từ có độ dài tối đa là 10 và chỉ có các chữ cái tiếng Anh viết thường. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. Bất kỳ giải pháp nào xử lý từng ký tự với số lần không đổi đều dễ dàng đủ. 

Trường hợp cạnh tinh tế là gốc tối thiểu. Từ “chúng tôi” tương ứng với một gốc rỗng. Việc xóa “us” sẽ tạo ra một chuỗi trống và số nhiều chính xác sẽ trở thành “i”. Một trường hợp góc khác là khi gốc tự kết thúc bằng các chữ cái giống với mẫu hậu tố bên trong, chẳng hạn như “sussus”. Chỉ có hai ký tự cuối cùng là quan trọng; sự xuất hiện trước đó của “chúng tôi” phải được bỏ qua. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là cố gắng phân tích cú pháp rõ ràng tất cả các phân tách gốc có thể có, nhưng điều đó là không cần thiết vì cấu trúc của ngôn ngữ rất cứng nhắc. Mọi đầu vào hợp lệ đều được đảm bảo kết thúc bằng “chúng tôi”, vì vậy quy tắc chuyển đổi có tính xác định. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm hoặc xác nhận bất cứ điều gì. Hậu tố được cố định nên gốc luôn lấy được bằng cách loại bỏ hai ký tự cuối. Sau khi đã trích xuất gốc, việc xây dựng số nhiều là nối trực tiếp với “i”. 

Điều này làm giảm vấn đề từ bất kỳ loại phân tích cú pháp hoặc khớp mẫu nào tới thao tác cắt chuỗi đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân tích cú pháp Brute Force | O(2^n) hoặc kết hợp không cần thiết | O(n) | Quá chậm/không cần thiết | 
| Loại bỏ hậu tố trực tiếp | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào đại diện cho một danh từ số ít. 
2. Xóa hai ký tự cuối của chuỗi. Điều này an toàn vì vấn đề đảm bảo hậu tố “chúng tôi” luôn tồn tại. 
3. Coi tiền tố còn lại là gốc của từ. 
4. Thêm ký tự “i” vào gốc để tạo thành dạng số nhiều. 
5. Xuất chuỗi kết quả. 

Bước lý luận duy nhất ở đây là việc loại bỏ hậu tố luôn hợp lệ và không phụ thuộc vào bất kỳ kiểm tra bổ sung nào. 

### Tại sao nó hoạt động 

Mọi từ hợp lệ đều được xây dựng dưới dạng root + “us”. Việc loại bỏ hai ký tự cuối cùng luôn tạo lại chính xác gốc ban đầu. Vì sự hình thành số nhiều được định nghĩa độc lập là gốc + “i”, phép biến đổi được xác định đầy đủ và nội từ, nghĩa là không tồn tại sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    w = input().strip()
    root = w[:-2]
    print(root + "i")
```Sau khi đọc từng từ, chúng ta chỉ cần cắt bỏ hai ký tự cuối cùng. Điều này hoạt động an toàn vì đảm bảo “kết thúc với chúng tôi” đảm bảo lát cắt không bao giờ làm hỏng thư mục gốc. Sau đó chúng ta nối “i” để tạo thành số nhiều. Việc triển khai sẽ tránh mọi điều kiện hoặc trường hợp đặc biệt vì ngay cả trường hợp gốc trống cũng hoạt động chính xác: “us” trở thành “” và sau đó là “i”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"fungus"`| Bước | Giá trị | 
| --- | --- | 
| bản gốc | nấm | 
| xóa hậu tố | nấm | 
| thêm tôi | nấm | 

Điều này cho thấy một trường hợp tiêu chuẩn trong đó gốc không trống và đơn giản. 

### Ví dụ 2 

đầu vào:`"us"`| Bước | Giá trị | 
| --- | --- | 
| bản gốc | chúng tôi | 
| xóa hậu tố | "" | 
| thêm tôi | tôi | 

Điều này thể hiện trường hợp cạnh của một gốc rỗng, xác nhận rằng việc cắt sẽ tạo ra một chuỗi trống một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case xử lý tối đa 10 ký tự | 
| Không gian | O(1) | Chỉ các lát chuỗi tạm thời được lưu trữ | 

Các ràng buộc rất nhỏ nên việc xử lý tuyến tính trên tất cả các trường hợp thử nghiệm là không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        w = sys.stdin.readline().strip()
        out.append(w[:-2] + "i")
    return "\n".join(out)

# provided samples
assert run("""9
us
sus
fungus
cactus
sussus
amogus
chungus
ntarsus
skibidus
""") == """i
si
fungi
cacti
sussi
amogi
chungi
ntarsi
skibidi"""

# custom cases
assert run("""3
us
bonus
us
""") == """i
boni
i"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chúng tôi | tôi | xử lý gốc trống | 
| tiền thưởng | boni | loại bỏ hậu tố thông thường | 
| chúng tôi/chúng tôi | tôi/tôi | trường hợp tối thiểu lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào “us”, việc loại bỏ hai ký tự cuối cùng sẽ tạo ra một chuỗi trống. Thuật toán xử lý chính xác điều này mà không cần logic đặc biệt và xuất ra “i”, khớp với dạng số nhiều được yêu cầu. 

Đối với bất kỳ từ nào có chứa chuỗi “chúng tôi” bên trong, chẳng hạn như “sussus”, chỉ có hai ký tự cuối cùng bị xóa. Gốc trở thành “suss” và thêm “i” sẽ tạo thành “sussi”, cho thấy các mẫu bên trong không ảnh hưởng đến tính chính xác.
