# function

- =IF(COUNTIF(�͈�,����),�^�̏ꍇ,�U�̏ꍇ)
  - ex
    - =IF(COUNTIF(A1,"*Excel*"),"��","")


- INDEX(�͈�,�c�ʒu[,���ʒu])

  - �͈͂̏c�ʒu�Ŏ������J�����̒l���擾����֐��B
  - �͈͂̏c�ʒu�Ɖ��ʒu�Ŏ������J�����̒l���擾����֐��B
  
- MATCH(�����l,�͈�,FLAG)

  - �͈͂̃J�����Ɉ�v����A�������͋ߎ�����l�̃J�����̈ʒu�����߂�֐�
  - FLAG
    - 0 : ��v
    - 1 or -1 : �ߎ��l

- example

  - INDEX( value_list, MATCH( A7, select_list, 0 ) )
    - select_list = ( apple, orange, banana )
    - value_list  = ( 100yen, 150yen, 50yen )

# TIPS

- �s�̍Ō�܂őI��
  - Shift + Ctrl + �����i����j

- Excel:�w�b�_�[�t�b�^�[�̈��
  - �u�y�[�W���C�A�E�g�v�u�y�[�W�ݒ�v�u����^�C�g���v�ň���^�C�g���s���w�肷��B
�@
- Excel format
  - You can use the Open XML Format for excel Practically your Excel is a zip file containing few other XML files.
  - http://msdn.microsoft.com/en-us/library/aa338205%28office.12%29.aspx#office2007aboutnewfileformat_introduction
