---
title: "CF 106136D - Vườn Khảm"
description: "Chúng tôi đang làm việc với một chuỗi cố định, ECUST và nhiệm vụ là tạo ra mọi chuỗi có thể có thể được hình thành bằng cách chọn độc lập kiểu chữ của từng ký tự trong khi vẫn giữ nguyên các chữ cái bên dưới."
date: "2026-06-20T01:53:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106136
codeforces_index: "D"
codeforces_contest_name: "East China University of Science and Technology Programming Contest 2025"
rating: 0
weight: 106136
solve_time_s: 55
verified: true
draft: false
---

[CF 106136D - Vườn khảm](https://codeforces.com/problemset/problem/106136/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một chuỗi cố định,`ECUST`và nhiệm vụ là tạo ra mọi chuỗi có thể được hình thành bằng cách chọn độc lập kiểu chữ của từng ký tự trong khi vẫn giữ nguyên các chữ cái bên dưới. Mỗi ký tự vẫn ở vị trí của nó, nhưng nó có thể là chữ hoa hoặc chữ thường và tất cả các kết hợp như vậy được coi là kết quả đầu ra hợp lệ. 

Đầu ra không phải là tùy ý. Mỗi chuỗi được tạo phải được sắp xếp theo thứ tự từ điển ASCII tiêu chuẩn. Thứ tự này so sánh các chuỗi ký tự theo ký tự và ở vị trí đầu tiên nơi chúng khác nhau, chuỗi có giá trị ASCII nhỏ hơn sẽ xuất hiện trước. Vì các chữ cái viết hoa có mã ASCII nhỏ hơn các chữ cái viết thường nên các phiên bản viết hoa của cùng một chữ cái luôn đứng trước các chữ cái viết thường khi so sánh ở cùng một vị trí. 

Mặc dù về mặt kỹ thuật, định dạng đầu vào cung cấp một dòng nhưng nó không mang thông tin nào ảnh hưởng đến câu trả lời. Toàn bộ vấn đề có tính xác định và chỉ phụ thuộc vào chuỗi`ECUST`. 

Ràng buộc tiềm ẩn là độ dài chuỗi được cố định ở mức 5, do đó tổng số chuỗi được tạo ra cũng được cố định ở mức$2^5 = 32$. Điều này ngay lập tức loại trừ mọi lo ngại về hiệu suất hoặc việc sử dụng bộ nhớ. Ngay cả một phép liệt kê đơn giản cũng nhanh chóng một cách tầm thường. 

Sự tinh tế chính nằm ở cách đặt hàng. Một cách tiếp cận đơn giản có thể tạo ra tất cả các kết hợp chữ hoa và sau đó sắp xếp chúng bằng cách sử dụng cách sắp xếp chuỗi có mục đích chung. Điều đó là an toàn, nhưng nó ẩn một thuộc tính cấu trúc cho phép tạo trực tiếp theo đúng thứ tự. 

Một sai lầm điển hình là giả sử thứ tự từ điển trong các tổ hợp chữ hoa hoạt động giống như số nhị phân mà không kiểm tra các quy tắc ASCII. Ví dụ: coi chữ thường là "nhỏ hơn" sẽ đảo ngược thứ tự đúng và tạo ra kết quả không chính xác. 

Một vấn đề tế nhị khác là quên rằng mỗi nhân vật đều thay đổi độc lập. Nếu ai đó chuyển đổi không chính xác chữ hoa chữ thường hoặc hoán đổi ký tự, họ sẽ không tạo ra các biến thể bảo toàn chữ hoa chữ thường. 

## Phương pháp tiếp cận 

Phương pháp brute-force là tạo ra tất cả các tập hợp con các vị trí thành chữ thường. Đối với mỗi ký tự trong số năm ký tự, chúng tôi quyết định xem nó vẫn là chữ hoa hay chữ thường, tạo ra 32 chuỗi. Sau khi tạo chúng, chúng tôi sắp xếp danh sách bằng cách sử dụng so sánh chuỗi thông thường. 

Điều này hoạt động vì không gian tìm kiếm cực kỳ nhỏ. Chi phí về cơ bản là tạo ra 32 chuỗi và sắp xếp chúng, không đáng kể. Tuy nhiên, nó thực hiện những công việc không cần thiết vì việc sắp xếp không thực sự cần thiết nếu chúng ta khai thác cách thứ tự ASCII tương tác với các thay đổi kiểu chữ. 

Quan sát quan trọng là mỗi vị trí hoạt động độc lập trong so sánh từ điển. Ở mỗi ký tự, chữ hoa luôn nhỏ hơn chữ thường đối với cùng một chữ cái. Điều này có nghĩa là nếu chúng ta hiểu chữ hoa là 0 và chữ thường là 1 thì thứ tự từ điển trên toàn bộ chuỗi khớp với thứ tự tăng dần tự nhiên của mặt nạ nhị phân 5 bit. 

Vì vậy, thay vì tạo và sắp xếp, chúng ta có thể liệt kê các mặt nạ từ 0 đến 31 và trực tiếp xây dựng chuỗi tương ứng. Điều này tự động tạo ra thứ tự chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force + Sắp xếp | O(32 log 32) | O(32) | Đã chấp nhận | 
| Đếm Bitmask | O(32) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta đối xử với từng nhân vật trong`ECUST`có hai trạng thái, chữ hoa và chữ thường và diễn giải một lựa chọn dưới dạng quyết định nhị phân. 

1. Xem xét tất cả các số nguyên từ 0 đến 31, trong đó mỗi số đại diện cho một sự kết hợp duy nhất của các lựa chọn cách viết hoa. Mỗi bit tương ứng với một vị trí trong chuỗi. Điều này hoạt động vì 5 ký tự đưa ra chính xác 5 quyết định nhị phân. 
2. Đối với mỗi số, hãy kiểm tra biểu diễn nhị phân của nó từ bit có trọng số cao nhất (vị trí 0) đến bit có trọng số thấp nhất (vị trí 4). Giá trị bit bằng 0 có nghĩa là chúng tôi giữ ký tự viết hoa và giá trị bit bằng 1 có nghĩa là chúng tôi chuyển đổi nó thành chữ thường. 
3. Xây dựng chuỗi ký tự kết quả theo từng ký tự. Tại mỗi vị trí, áp dụng quy tắc từ bước 2 để quyết định hạ thấp hay giữ ký tự. 
4. In ngay từng chuỗi đã tạo. Vì mặt nạ được xử lý theo thứ tự tăng dần nên kết quả đầu ra đã có thứ tự từ điển chính xác. 

Chi tiết quan trọng là việc lặp lại từ 0 trở lên đảm bảo tính chính xác về mặt từ điển vì các bit trước đó tương ứng với các ký tự trước đó và chữ hoa luôn nhỏ hơn về mặt từ điển so với chữ thường. 

### Tại sao nó hoạt động 

Thuật toán dựa trên sự liên kết thứ tự nhất quán giữa hai hệ thống: thứ tự nhị phân của mặt nạ và thứ tự từ điển ASCII của chuỗi. Tại vị trí khác nhau đầu tiên giữa hai chuỗi được tạo, vị trí đó tương ứng chính xác với bit khác biệt đáng kể nhất trong mặt nạ của chúng. Vì chúng ta ánh xạ 0 thành chữ hoa và 1 thành chữ thường và chữ hoa có giá trị ASCII nhỏ hơn nên thứ tự của mặt nạ khớp trực tiếp với thứ tự từ điển. Bất biến này đảm bảo rằng không cần sắp xếp sau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = "ECUST"
n = len(s)

for mask in range(1 << n):
    res = []
    for i in range(n):
        c = s[i]
        if mask & (1 << (n - 1 - i)):
            res.append(c.lower())
        else:
            res.append(c)
    print("".join(res))
```Đầu tiên, chương trình sửa chuỗi cơ sở và lặp lại tất cả các mặt nạ bit có thể thể hiện các lựa chọn vỏ. Kiểm tra bit được căn chỉnh sao cho bit quan trọng nhất tương ứng với ký tự đầu tiên, đảm bảo tính chính xác về mặt từ điển. 

Việc xây dựng chuỗi được thực hiện trong một lượt cho mỗi mặt nạ, giúp việc triển khai đơn giản và tránh mọi nhu cầu sắp xếp logic hoặc cấu trúc dữ liệu phụ trợ. 

## Ví dụ đã hoạt động 

Chúng ta có thể minh họa quá trình trên một chuỗi tương tự nhỏ hơn`AB`, trong đó logic dễ nhìn hơn trong khi vẫn giữ nguyên cấu trúc. 

Vì`AB`, chúng tôi liệt kê các mặt nạ từ 0 đến 3. 

| Mặt nạ | Nhị phân | Một sự lựa chọn | Lựa chọn B | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 00 | A | B | AB | 
| 1 | 01 | A | b | Ab | 
| 2 | 10 | một | B | aB | 
| 3 | 11 | một | b | ab | 

Dấu vết này cho thấy thứ tự mặt nạ tăng dần phù hợp với thứ tự từ điển vì ở ký tự khác nhau đầu tiên, chữ hoa luôn đứng trước chữ thường. 

Áp dụng lý luận tương tự cho`ECUST`chỉ cần mở rộng mẫu này đến năm vị trí, tạo ra 32 đầu ra theo thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(32 × 5) | Mỗi mặt nạ trong số 32 mặt nạ tạo thành một chuỗi 5 ký tự | 
| Không gian | O(1) thêm | Bỏ qua đầu ra, chỉ một bộ đệm nhỏ cho mỗi chuỗi được sử dụng | 

Kích thước đầu vào không đổi, do đó giải pháp dễ dàng phù hợp với mọi giới hạn hợp lý. Ngay cả với nhiều định dạng thử nghiệm, quá trình tính toán vẫn diễn ra tức thời một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = "ECUST"
    n = len(s)
    out = []
    for mask in range(1 << n):
        res = []
        for i in range(n):
            c = s[i]
            if mask & (1 << (n - 1 - i)):
                res.append(c.lower())
            else:
                res.append(c)
        out.append("".join(res))
    return "\n".join(out)

# sample-style check (structure only)
assert len(run("NO_INPUT").splitlines()) == 32

# custom case: single letter analogue logic check
def run_small():
    s = "A"
    return [("A" if m == 0 else "a") for m in range(2)]

assert run_small() == ["A", "a"]

# ordering check for ECUST prefix property
res = run("NO_INPUT").splitlines()
assert res[0] == "ECUST"
assert res[-1] == "ecust"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| KHÔNG_INPUT | 32 dây | kích thước liệt kê đầy đủ | 
| mẫu chữ đơn | Một, một | trường hợp chuyển đổi chính xác | 
| Phạm vi trường hợp ECUST | ECUST trước, ecu sau | từ điển cực đoan | 

## Vỏ cạnh 

Hành vi cạnh có ý nghĩa duy nhất là sắp xếp tại các ranh giới: các chuỗi hoàn toàn viết hoa và viết thường. 

Đối với đầu vào`ECUST`, mặt nạ`00000`sản xuất`ECUST`, là chuỗi nhỏ nhất vì mọi ký tự đều là chữ hoa. Mặt nạ`11111`sản xuất`ecust`, lớn nhất vì mọi ký tự đều là chữ thường. 

Đi qua trường hợp đầu tiên, mỗi bit bằng 0, do đó không có ký tự nào được chuyển đổi và đầu ra vẫn giữ nguyên`ECUST`. Vì không có chuỗi nào khác có thể có ký tự nhỏ hơn ở vị trí khác nhau đầu tiên nên nó phải xuất hiện đầu tiên theo thứ tự từ điển. 

Đối với trường hợp cuối cùng, tất cả các bit là một, vì vậy mọi ký tự đều là chữ thường. Bất kỳ chuỗi nào khác khác trước đó hoặc ở một vị trí nào đó mà chữ hoa xuất hiện và chữ hoa nhỏ hơn, do đó tất cả các chuỗi như vậy đứng trước nó, làm cho`ecust`đầu ra cuối cùng.
