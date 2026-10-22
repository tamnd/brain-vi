---
title: "CF 105900D - Mê sảng tại Unballoon"
description: "Chúng ta đang so sánh hai hộp chứa đầy kem: một chai hình trụ và một hộp hình nón có thêm một mái vòm ở trên. Chai là một hình trụ đơn giản có bán kính rL và chiều cao hL, được đổ đầy hoàn toàn. Dung tích của nó chỉ bằng thể tích xi lanh thông thường."
date: "2026-06-21T15:17:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "D"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 50
verified: true
draft: false
---

[CF 105900D - Mê sảng tại Unballoon](https://codeforces.com/problemset/problem/105900/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang so sánh hai hộp chứa đầy kem: một chai hình trụ và một hộp hình nón có thêm một mái vòm ở trên. 

Cái chai là một hình trụ đơn giản có bán kính`rL`và chiều cao`hL`, được lấp đầy hoàn toàn. Dung tích của nó chỉ bằng thể tích xi lanh thông thường. 

Cạnh hình nón gồm một hình nón tròn bên phải có bán kính`rC`và chiều cao`hC`, cũng được lấp đầy hoàn toàn, và khi đó một bán cầu đặc có cùng bán kính`rC`được đặt lên trên và cũng chứa đầy kem. Vì vậy, hộp đựng hình nón thực sự có hai phần góp phần tạo nên thể tích. 

Nhiệm vụ là quyết định xem chai có chứa được nhiều kem hơn cấu trúc hình nón với bán cầu hay không. Nếu có, chúng tôi xuất ra`Injusto`, nếu không thì chúng tôi xuất ra`Justo`. 

Kích thước đầu vào rất nhỏ, tất cả các kích thước tối đa là 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa nâng cao. Tính toán trực tiếp theo thời gian không đổi là đủ và thậm chí số học dấu phẩy động cũng sẽ hoạt động về mặt kỹ thuật, mặc dù nó gây ra rủi ro về độ chính xác không cần thiết. 

Vấn đề tinh tế duy nhất là các thể tích liên quan đến π, nhưng vì cả hai vế đều bao gồm π là một thừa số nên nó sẽ bị loại trừ hoàn toàn nếu chúng ta so sánh bằng đại số. Việc triển khai bất cẩn giữ số π và sử dụng so sánh dấu phẩy động vẫn có thể vượt qua, nhưng có thể tránh được và kém hiệu quả hơn. 

Các trường hợp cạnh chủ yếu là về sự so sánh bình đẳng và ranh giới. Ví dụ: khi cả hai container có dung lượng giống nhau thì câu trả lời phải là`Justo`vì điều kiện đòi hỏi chai phải lớn hơn một cách nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ tính toán trực tiếp cả hai tập bằng cách sử dụng các công thức hình học. Cái chai rất đơn giản: một thể tích hình trụ`π rL^2 hL`. Hình nón là tổng thể tích của hình nón`(1/3)π rC^2 hC`và thể tích bán cầu`(2/3)π rC^3`. 

Cách tiếp cận này đã là O(1), do đó không có tắc nghẽn thuật toán. Sự kém hiệu quả tiềm ẩn duy nhất là việc sử dụng không cần thiết số học dấu phẩy động và số π, điều này gây ra các vấn đề làm tròn khi so sánh các giá trị phải là số nguyên chính xác sau khi chia tỷ lệ. 

Quan sát quan trọng là π chung cho mọi số hạng. Nếu chúng ta nhân mọi thứ với 3/π, chúng ta sẽ chuyển đổi tất cả các tập thành số nguyên: 

cái chai trở thành`3 rL^2 hL`, và hình nón trở thành`rC^2 hC + 2 rC^3`. Sau phép biến đổi này, việc so sánh hoàn toàn dựa trên số nguyên và chính xác. 

Điều này làm giảm vấn đề thành một so sánh số nguyên duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hình học trực tiếp với phao | O(1) | O(1) | Hoạt động nhưng rủi ro chính xác | 
| So sánh số nguyên tỷ lệ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Đọc bốn số nguyên`rL, hL, rC, hC`. Chúng xác định hai khối hình học có thể tích mà chúng ta sẽ so sánh. 
2. Tính thể tích chai đã chia tỷ lệ như sau`Vb = 3 * rL * rL * hL`. Điều này xuất phát từ việc nhân thể tích hình trụ với 3 và loại bỏ số π. 
3. Tính thể tích hình nón và bán cầu theo tỷ lệ như sau`Vc = rC * rC * hC + 2 * rC * rC * rC`. Số hạng đầu tiên tương ứng với hình nón, số hạng thứ hai tương ứng với bán cầu sau khi chia tỷ lệ. 
4. So sánh`Vb`Và`Vc`. Nếu như`Vb`thực sự lớn hơn, sản lượng`Injusto`. Nếu không thì xuất ra`Justo`. 

Lý do đằng sau việc chia tỷ lệ là nó duy trì thứ tự chính xác trong khi loại bỏ các hằng số vô tỷ. 

### Tại sao nó hoạt động 

Cả hai thể tích ban đầu đều tỷ lệ với π. Vì π dương và giống nhau trong cả hai biểu thức nên việc nhân hoặc chia cho π không làm thay đổi thứ tự. Tương tự, nhân mọi thứ với 3 sẽ xóa phân số trong công thức hình nón. Sau phép biến đổi này, cả hai biểu thức đều trở thành số nguyên biểu thị chính xác các thể tích tỷ lệ, do đó việc so sánh chúng về mặt toán học tương đương với việc so sánh các thể tích ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    rL, hL, rC, hC = map(int, input().split())

    bottle = 3 * rL * rL * hL
    cone = rC * rC * hC + 2 * rC * rC * rC

    if bottle > cone:
        print("Injusto")
    else:
        print("Justo")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo sự đơn giản hóa đại số một cách trực tiếp. Việc chăm sóc duy nhất cần thiết là sử dụng nhất quán số học số nguyên. Tất cả các giá trị trung gian vừa vặn thoải mái trong số nguyên 32 bit vì cường độ tối đa nằm trong khoảng`3 * 100^3 = 3e6`. 

Bước quyết định sử dụng bất đẳng thức nghiêm ngặt theo yêu cầu của câu lệnh. Bất kỳ ràng buộc nào mặc định là`Justo`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 4 1 10
```| Bước | Giá trị chai`3 rL^2 hL`| Giá trị hình nón`rC^2 hC + 2 rC^3`| Quyết định | 
| --- | --- | --- | --- | 
| Tính giá trị | 3 * 1 * 1 * 4 = 12 | 1*1*10 + 2*1 = 12 | bằng | 

Vì các giá trị bằng nhau nên chai không hẳn là lớn hơn, nên sản lượng là`Justo`. 

Điều này cho thấy trường hợp đẳng thức trong đó cả hai hình đều lưu trữ chính xác cùng một lượng sau khi chuẩn hóa. 

### Ví dụ 2 

đầu vào:```
78 57 60 40
```| Bước | Giá trị chai | Giá trị hình nón | Quyết định | 
| --- | --- | --- | --- | 
| Tính giá trị | 3 * 78^2 * 57 = 1041192 | 60^2 * 40 + 2 * 60^3 = 864000 | chai lớn hơn | 

Vì giá trị chai vượt quá giá trị hình nón nên sản lượng là`Injusto`. 

Điều này chứng tỏ trường hợp số hạng bậc ba trong hình nón lớn nhưng vẫn bị hình trụ chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học trên bốn số nguyên | 
| Không gian | O(1) | Không sử dụng công trình phụ trợ | 

Việc tính toán diễn ra theo thời gian không đổi và phù hợp một cách tầm thường trong các ràng buộc. Ngay cả với nhiều trường hợp thử nghiệm, lý do tương tự vẫn sẽ mở rộng tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    rL, hL, rC, hC = map(int, sys.stdin.readline().split())

    bottle = 3 * rL * rL * hL
    cone = rC * rC * hC + 2 * rC * rC * rC

    print("Injusto" if bottle > cone else "Justo")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1 4 1 10\n") == "Justo"
assert run("78 57 60 40\n") == "Injusto"

# custom cases
assert run("1 1 1 1\n") == "Justo", "tie case"
assert run("2 100 1 1\n") == "Injusto", "large bottle dominance"
assert run("1 1 5 5\n") == "Justo", "large cone dominance"
assert run("10 10 10 10\n") == "Justo", "balanced larger values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | Justo | xử lý bình đẳng | 
| 2 100 1 1 | Bất công | sự thống trị chai lớn | 
| 1 1 5 5 | Justo | hình nón chiếm ưu thế thông qua thuật ngữ bán cầu | 
| 10 10 10 10 | Justo | trường hợp vừa đối xứng | 

## Vỏ cạnh 

Một trường hợp quan trọng là sự bình đẳng sau khi chia tỷ lệ. Ví dụ,`1 4 1 10`tạo ra khối lượng có tỷ lệ giống hệt nhau. Thuật toán tính toán chính xác cả hai biểu thức và rơi vào`Justo`nhánh vì điều kiện nghiêm ngặt. 

Một trường hợp khác là bán kính hình nón lớn nhưng chiều cao nhỏ. Mặc dù số hạng bán cầu tăng theo khối, cái chai vẫn có thể chiếm ưu thế nếu bán kính hoặc chiều cao của nó đủ lớn. Việc so sánh số nguyên xử lý việc này mà không có bất kỳ sự mất ổn định nào. 

Kịch bản cạnh cuối cùng là giá trị tối đa khoảng 100. Ngay cả khi đó, giá trị trung gian lớn nhất là khoảng`3 * 100^3 = 3,000,000`, nằm trong giới hạn số nguyên, do đó không phát sinh vấn đề tràn hoặc độ chính xác.
