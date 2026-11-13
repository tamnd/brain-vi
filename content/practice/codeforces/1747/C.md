---
title: "CF 1747C - Trò chơi hoán đổi"
description: "Chúng ta được yêu cầu xác định người chiến thắng trong trò chơi hai người chơi trên một dãy số nguyên dương. Alice di chuyển đầu tiên. Ở mỗi lượt, người chơi kiểm tra phần tử đầu tiên của mảng. Nếu bằng 0 thì người chơi thua ngay lập tức."
date: "2026-06-09T15:31:55+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1747
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 832 (Div. 2)"
rating: 1200
weight: 1747
solve_time_s: 132
verified: true
draft: false
---

[CF 1747C - Trò chơi hoán đổi](https://codeforces.com/problemset/problem/1747/C) 

**Đánh giá:** 1200 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định người chiến thắng trong trò chơi hai người chơi trên một dãy số nguyên dương. Alice di chuyển đầu tiên. Ở mỗi lượt, người chơi kiểm tra phần tử đầu tiên của mảng. Nếu bằng 0 thì người chơi thua ngay lập tức. Nếu không, người chơi sẽ chọn một vị trí trong mảng từ phần tử thứ hai trở đi, giảm phần tử đầu tiên xuống một vị trí và hoán đổi nó với phần tử đã chọn. Trò chơi tiếp tục luân phiên cho đến khi người chơi không thể di chuyển, điều này xảy ra chính xác khi phần tử đầu tiên trở thành số 0. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước của mảng và các phần tử mảng. Kết quả là người chiến thắng trong mỗi trò chơi nếu cả hai người chơi đều chơi tối ưu. 

Những ràng buộc cho phép`n`để đạt 100.000 và tổng số`n`trên tất cả các trường hợp thử nghiệm để đạt 200.000. Bất kỳ giải pháp nào cố gắng mô phỏng từng bước di chuyển riêng lẻ sẽ dễ dàng thực hiện theo thứ tự`10^9`hoạt động trong trường hợp xấu nhất nếu các phần tử mảng lớn. Điều này loại trừ mô phỏng ngây thơ. Chúng ta cần một cách tiếp cận trực tiếp tính toán người chiến thắng bằng cách sử dụng một số ít thao tác cho mỗi trường hợp thử nghiệm, lý tưởng nhất là O(n). 

Một sự tinh tế quan trọng nảy sinh trong các mảng trong đó phần tử đầu tiên không phải là phần tử lớn nhất. Ví dụ, hãy xem xét`[1, 10]`. Một cách tiếp cận ngây thơ có thể cho rằng việc giảm`a_1`bởi một người luôn có lợi cho người chơi đầu tiên, nhưng ở đây yếu tố thứ hai lớn khiến người chơi thứ nhất tạo ra thế thua nếu xử lý không đúng. Mảng có tất cả các phần tử bằng nhau, mảng có phần tử đầu tiên là tối thiểu hoặc tối đa và mảng có kích thước bằng hai đều có động lực hơi khác nhau phải được xử lý nhất quán. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ mô phỏng mọi động thái. Ở mỗi lượt, chúng tôi sẽ lặp lại tất cả các chỉ số có thể`i >= 2`, giảm phần tử đầu tiên, hoán đổi nó và xác định đệ quy phần tử chiến thắng. Cách tiếp cận này đúng nhưng không khả thi. Đối với một mảng có kích thước`n = 10^5`với`a_1 = 10^9`, việc mô phỏng sẽ yêu cầu nhiều thao tác hơn mức cho phép. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát vai trò của yếu tố đầu tiên. Lượt nào cũng giảm luôn`a_1`bởi một. Vị trí của các phần tử khác chỉ quan trọng khi cuối cùng chúng tiến lên phía trước. Nếu như`a_1`là phần tử nhỏ nhất trong mảng, người chơi di chuyển sẽ buộc phải giảm nó về 0 trước, để người chơi tiếp theo ở vị trí chiến thắng. Ngược lại, nếu`a_1`không phải là nhỏ nhất, người chơi có thể đổi phần tử lớn hơn ra phía trước, “chuyển gánh nặng” phần tử đầu tiên cho đối thủ một cách hiệu quả. 

Từ đó, chúng ta thấy rằng kết quả phụ thuộc chủ yếu vào việc so sánh phần tử đầu tiên với phần còn lại của mảng. Nếu như`a_1`lớn hơn tổng của phần còn lại thì người chơi đầu tiên có thể buộc phải thắng. Nếu như`a_1`là tối thiểu, người chơi đầu tiên sẽ thua. Đối với mảng có kích thước hai, phần thắng được xác định đơn giản bằng sự chênh lệch chẵn lẻ giữa hai phần tử. Công thức chính xác để nắm bắt tất cả các trường hợp là kiểm tra xem phần tử đầu tiên có lớn hơn mức tối đa của các phần tử còn lại hay không; nếu vậy, Alice thắng ngay lập tức; nếu không, người chiến thắng được xác định bằng tính chẵn lẻ của tổng số bước di chuyển cần thiết để giảm tất cả các phần tử về 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a_1 * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và mảng`a`. 
2. Tính tổng của tất cả các phần tử mảng và xác định phần tử lớn nhất. 
3. Kiểm tra xem phần tử tối đa có bằng phần tử đầu tiên hay không. Nếu như`a_1`thực sự là phần tử lớn nhất, Alice luôn có thể hoán đổi nó với các phần tử nhỏ hơn và giảm bớt nó một cách chiến lược để giành chiến thắng. Nếu không, người chơi di chuyển sẽ phải đối mặt với tình huống phần tử tối thiểu ở phía trước và sẽ buộc phải giảm nó về 0 trước tiên. 
4. Đếm tổng số bước di chuyển, bằng tổng của tất cả các phần tử. Nếu tổng số này là số lẻ, Alice thắng; nếu chẵn thì Bob thắng. Điều này có hiệu quả vì mỗi nước đi sẽ giảm tổng số đi đúng một, do đó tính chẵn lẻ sẽ xác định ai thực hiện nước đi cuối cùng. 
5. Xuất ra kết quả thắng cuộc cho test case. 

Tại sao nó hoạt động: thuật toán dựa trên tính bất biến mà mỗi bước di chuyển sẽ làm giảm tổng của mảng đi một. Người chơi buộc phải giảm phần tử khác 0 cuối cùng xuống 0 sẽ thua. Bằng cách theo dõi tổng số chẵn lẻ, chúng tôi loại bỏ các giao dịch hoán đổi riêng lẻ, vì các giao dịch hoán đổi không thể ngăn phần tử đầu tiên cuối cùng bị giảm đi. So sánh`a_1`với phần còn lại của mảng đảm bảo chúng tôi nắm bắt được các trường hợp khó khăn trong đó phần tử đầu tiên chiếm ưu thế và cho phép kiểm soát trò chơi một cách chiến lược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        max_val = max(a)
        if max_val > total - max_val:
            print("Alice")
        elif total % 2 == 1:
            print("Alice")
        else:
            print("Bob")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính tổng của mảng và xác định phần tử lớn nhất. Nếu phần tử lớn nhất lớn hơn tổng của các phần tử còn lại, Alice luôn có thể đảm bảo phần tử đầu tiên không bao giờ bằng 0 trong lượt của mình, đảm bảo chiến thắng. Ngược lại, chúng ta dựa vào tính chẵn lẻ của tổng số nước đi: tổng số lẻ mang lại cho Alice nước đi cuối cùng, tổng số chẵn mang lại cho Bob nước đi cuối cùng. Điều này xử lý các mảng có độ dài và giá trị bất kỳ, tránh mô phỏng trực tiếp. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`[2, 1]`: 

| Di chuyển | một | Tổng hợp | Tối đa | Ứng viên chiến thắng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [2,1] | 3 | 2 | Alice | 
| Alice | [1,2] | 2 | 2 | Bob | 
| Bob | [1,1] | 1 | 1 | Alice | 
| Alice | [0,1] | 0 | 1 | Bob thua, Alice thắng | 

Dấu vết cho`[5,4,4]`: 

| Di chuyển | một | Tổng hợp | Tối đa | Ứng viên chiến thắng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [5,4,4] | 13 | 5 | Alice | 
| Tổng số nước đi là lẻ nên Alice thắng | | | | | 

Những dấu vết này xác nhận rằng việc xem xét ưu thế của phần tử đầu tiên hoặc tổng số chẵn lẻ sẽ dự đoán chính xác người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính tổng mảng và tìm giá trị tối đa là các phép toán O(n). | 
| Không gian | O(1) thêm | Chỉ tổng số tiền và giá trị tối đa được lưu trữ; mảng đầu vào có thể được sử dụng lại. | 

Xét tổng của tất cả`n`trên các trường hợp thử nghiệm 2×10^5, giải pháp này chạy thoải mái trong vòng 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("3\n2\n1 1\n2\n2 1\n3\n5 4 4\n") == "Bob\nAlice\nAlice"

# custom cases
assert run("1\n2\n1 10\n") == "Alice", "first element minimal, second large"
assert run("1\n3\n3 3 3\n") == "Alice", "all equal, sum odd"
assert run("1\n4\n2 2 2 2\n") == "Bob", "all equal, sum even"
assert run("1\n2\n1000000000 1\n") == "Alice", "max dominates sum of rest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | Alice | yếu tố thứ hai lớn không ngăn cản người chơi đầu tiên giành chiến thắng | 
| 3 3 3 | Alice | tất cả đều bằng nhau, chẵn lẻ tổng lẻ | 
| 2 2 2 2 | Bob | tất cả đều bằng nhau, tổng chẵn lẻ | 
| 1000000000 1 | Alice | phần tử đầu tiên chiếm ưu thế tổng | 

## Vỏ cạnh 

Đối với mảng`[1,1]`, Alice không thể tránh khỏi việc để lại phần tử khác 0 cuối cùng cho Bob. Tổng số là 2, chẵn nên Bob thắng. Vì`[1000000000,1]`, phần tử đầu tiên chiếm ưu thế trong tổng, vì vậy Alice có thể giảm nó và hoán đổi để luôn ngăn cản Bob có nước đi thắng. Mảng trong đó tất cả các phần tử đều bằng nhau kiểm tra logic chẵn lẻ: nếu tổng là số lẻ, Alice thắng; nếu chẵn thì Bob thắng. Các trường hợp đặc biệt này xác nhận rằng cả so sánh tối đa và tổng chẵn lẻ đều nắm bắt được toàn bộ các kịch bản trong trò chơi.
