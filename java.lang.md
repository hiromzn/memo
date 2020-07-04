# hashmap

http://www.ibm.com/developerworks/jp/java/library/j-jtp07233/index.html


���s�R���N�V�����E�N���X

ConcurrentHashMap��CopyOnWriteArrayList�ɂ��A�X���b�h�E�Z�[�t�ƃX�P�[���r���e�B�[�̉��ǂ������܂�
	developerWorks
	
	
�y�[�W�I�v�V����
	�v�����^�[������������ɃZ�b�g���Ă�������	

�y�[�W���������
	�y�[�W��e-���[���ő��M	

�y�[�W��e-���[���ő��M
	�����͂����� 	

�����͂�����

���x��: ����

Brian Goetz (brian@quiotix.com), Principal Consultant, Quiotix

2003�N 7�� 23��

    �������̑��֗̕��ȕ��s���r���f�B���O�E�u���b�N�ɉ����āADoug Lea����util.concurrent�p�b�P�[�W�ɂ́A�R���N�V�����E�^�C�v��List��Map�𗘗p�������@�\�ŃX���b�h�E�Z�[�t�ȏ����n���܂܂�Ă��܂����A����ABrian Goetz���́AHashtable��synchronizedMap��ConcurrentHashMap�ɕς��邾���ŁA���s�v���O�����͂ǂꂾ�����b�𓾂邱�Ƃ��ł��邩�ɂ��Đ������Ă��܂��B

Java�N���X�E���C�u�����[�̍ŏ��̌����I�R���N�V�����E�N���X�́AJDK 1.0�̋@�\�ł���Hashtable�ł����BHashtable�͎g���₷���ăX���b�h�E�Z�[�t�Ȍ����I�}�b�v�@�\��ێ����Ă���A�m���ɕ֗��Ȃ��̂ł����B�������AHashtable�̃X���b�h�E�Z�[�t�@�\�͂��ׂẴ��\�b�h�������������Ƃ����_�ŁA���Ȃ�s�ւȂ��̂ł����B�����JDK1.0�ł̋����̂Ȃ��������ɂ́A���Ȃ�̃p�t�H�[�}���X�E�R�X�g���������Ă��܂����BHashtable�̌�p�ł���HashMap��JDK 1.2�̃R���N�V�����E�t���[�����[�N�̋@�\�Ƃ��ēo�ꂵ�A�񓯊��̊��N���X����ѓ��������b�p�[�ł���Collections.synchronizedMap�ŁA�X���b�h�E�Z�[�t�ɑΏ����܂����B�܂�A�X���b�h�E�Z�[�t��Collections.synchronizedMap�����{�@�\�𕪊����邱�ƂŁA���������K�v�ȃ��[�U�[�͋@�\��L���A���������K�v�ł͂Ȃ����[�U�[�͋@�\��L���Ȃ��悤�ɂ��邱�Ƃ��ł���悤�ɂȂ�܂����B

Hashtable��synchronizedMap(Hashtable���邢�͓�����Map���b�p�[�I�u�W�F�N�g�̊e���\�b�h�𓯊������܂�)�ɂ�铯�����̃A�v���[�`�́A2�̏d�v�Ȗ�������Ă��܂��B1�ڂ́A1�x�Ƀn�b�V���E�e�[�u���ɃA�N�Z�X���邱�Ƃ��ł���X���b�h��1�����ł���Ƃ����X�P�[���r���e�B�[�̏�Q�ł��B2�ڂ́A������common�����I�y���[�V�������ǉ��̓�����v�����邢���_�ŁA�^�̃X���b�h�E�Z�[�t��񋟂��Ă���Ƃ͌����Ȃ��Ƃ������Ƃł��Bget()��put()�̂悤�ȃV���v���ȃI�y���[�V�����͒ǉ������Ȃ��ł��x��͂Ȃ����̂́A�C�e���[�V������Aput-if-absent�Ƃ����悤�ȃI�y���[�V�����̋��ʃV�[�P���X�́A�f�[�^�����̉�����O�������ɗv�����܂��B

�����t���X���b�h�E�Z�[�t

�������R���N�V�����E���b�p�[��synchronizedMap��synchronizedList�́A�����t���X���b�h�E�Z�[�t �ƌĂ΂�邱�Ƃ�����܂��B�ʂ̑���͂��ׂăX���b�h�E�Z�[�t�ł�����̂́A����t���[���O�̃I�y���[�V�����̌��ʂɈˑ�����悤�ȃI�y���[�V�����E�V�[�P���X�̏ꍇ�́A�f�[�^�������N����\�������邽�߂ł��B���X�g�P�̑O���́A�悭����uput-if-absent �Z�@�v�ł���A�G���g���[��Map�ɑ��݂��Ȃ��ꍇ�͒ǉ�����Ƃ������̂ł����A�����ɂ��A���X�g1�ɏ�����Ă���containsKey()�̃��\�b�h�̖߂��put()���\�b�h���Ă΂��܂ł̊Ԃɕʂ̃X���b�h������key�ɒl��ݒ肷�邱�Ƃ��\�ł��B�������݂���x�����ɂ�������΁AMap m�œ��������铯�����u���b�N�ŃX�e�[�g�����g�����b�v����K�v������܂��B

���X�g1�ł́A�C�e���[�V�����������Ă��܂��B�ŏ��̗�ł́A�ʂ̃X���b�h�����X�g����A�C�e�����폜���邱�Ƃ��ł����̂ŁA���[�v�̎��s����List.size()�̌��ʂ͖����ɂȂ�\��������܂����B�^�C�~���O�������[�v�̍Ō�̃C�e���[�V��������͂�������ɁA�A�C�e�����ʂ̃X���b�h�ɂ���č폜���ꂽ�ꍇ�AList.get()��null��Ԃ��A���̌���doSomething()��NullPointerException�𓊂��邱�ƂɂȂ�ł��傤�B�����������邽�߂ɂ͉����ł���ł��傤�H����List���g���ČJ�Ԃ����������Ă���ԂɁA�ʂ̃X���b�h��List�ɃA�N�Z�X����\��������Ƃ�����A�������u���b�N��List�����b�v��List l�𓯊������āA�J�Ԃ��̊Ԃ�List�S�̂����b�N���Ȃ���΂Ȃ�܂���B����ɂ��f�[�^�������������邱�Ƃ͂ł��܂����A�J�Ԃ��̊�List�S�̂����b�N����ƁA���̃X���b�h�͒�����List�ɃA�N�Z�X���邷�邱�Ƃ��ł��Ȃ��Ȃ�A���s���ɐ[���ȉe����^���邱�ƂɂȂ��Ă��܂��܂��B

�R���N�V�����E�t���[�����[�N�̓��X�g���邢�͑��̃R���N�V�����̌����ɃC�e���[�^�𓱓����܂����B����ɂ��A�R���N�V�����̗v�f��ʂ��ČJ�Ԃ��������ő���ɗ��p���邱�Ƃ��ł��܂��B�������Ajava.util��Collections�N���X�Ŏ������ꂽ�C�e���[�^��fail-fast�ł���̂ŁA����X���b�h��Iterator��ʂ��Č������Ă���Ԃɕʂ̃X���b�h���R���N�V������ύX����΁A���̌��Iterator.hasNext()��������Iterator.next()�ďo����ConcurrentModificationException�𓊂���Ƃ������ƂɂȂ�܂��B��̗�̂悤�ɁAConcurrentModificationException�������������΁AList l�œ��������铯�����u���b�N��List�S�̂����b�v���A�J�Ԃ��̊�List�S�̂����b�N���Ȃ���΂Ȃ�܂���B(�܂��́A��������K�v�Ƃ��Ȃ��z���List.toArray()��iterate���N�����邱�Ƃ��ł��܂����A���X�g���傫���ꍇ�̓p�t�H�[�}���X�E�R�X�g���������Ă��܂��܂��B)

���X�g1. synchronized map�̂悭���鋣�����

                
    Map m = Collections.synchronizedMap(new HashMap());
    List l = Collections.synchronizedList(new ArrayList());
    // put-if-absent idiom -- contains a race condition
    // may require external synchronization
    if (!map.containsKey(key))
      map.put(key, value);
    // ad-hoc iteration -- contains race conditions
    // may require external synchronization
    for (int i=0; i<list.size(); i++) {
      doSomething(list.get(i));
    }
    // normal iteration -- can throw ConcurrentModificationException
    // may require external synchronization
    for (Iterator i=list.iterator(); i.hasNext(); ) {
      doSomething(i.next());
    }


�M���Ɋւ��������ӎ�

synchronizedList��synchronizedMap���񋟂��Ă�������t���X���b�h�E�Z�[�t�ɂ́A�B���ꂽ�댯��������܂��B�J���҂͂����̃R���N�V�����͓��������Ă���A���S�ɃX���b�h�E�Z�[�t�ł���ƍl���A�����I�ȃI�y���[�V������K�؂ɓ����������邱�Ƃ�ӂ���悤�ɂȂ�̂ł��B���̌��ʁA�����̃v���O�����͊ȒP�ɋ@�\���Ă���悤�Ɍ����āA���ۂ͍������ׂɂ��ANullPointerException��ConcurrentModificationException�𓊂��邱�ƂɂȂ邩������Ȃ��̂ł��B



	��ɖ߂�


�X�P�[���r���e�B�[�̖��

�X�P�[���r���e�B�[�Ƃ́A�A�v���P�[�V�����̃��[�N���[�h�◘�p�ł���R���s���[�^�E���\�[�X�̑����ɉ����āA�X���[�v�b�g���ǂ̂悤�ɂȂ邩�Ƃ������Ƃł��B�X�P�[���u���ȃv���O�����́A�v���Z�b�T�[�A�������[�AI/O�ш敝�ɔ�Ⴕ�āA���傫�ȃ��[�N���[�h���������Ƃ��ł��܂��B�r���A�N�Z�X�p�̋��L���\�[�X�����b�N���邱�Ƃ́A�X�P�[���r���e�B�[�̃{�g���l�b�N�ł��B�Ƃ����̂��A���Ƃ��A�C�h���E�v���Z�b�T�[���X���b�h�g�p�ɑg�ݓ���邱�Ƃ��ł����Ƃ��Ă��A���̑��̃X���b�h�̓��\�[�X�ɃA�N�Z�X���邱�Ƃ��ł��Ȃ�����ł��B�X�P�[���r���e�B�[���������邽�߂ɂ́A�r���I�ȃ��\�[�X�E���b�N�ւ̈ˑ���r�����邩�k�����Ȃ���΂Ȃ�܂���B

�������R���N�V�����E���b�p�[�Ɋւ���d�v�Ȗ��́AHashtable��Vector�N���X�����̃��b�N�œ���������Ƃ������Ƃł��B�܂�A1�x��1�̃X���b�h�������R���N�V�����ɃA�N�Z�X�ł��A1�̃X���b�h��Map����ǂ݂��ޓr���ł���ꍇ�A�ǂݍ��݂⏑�����݂��s���������̂��ׂẴX���b�h�͑ҋ@���Ȃ���΂Ȃ�܂���B�ł���ʓI��Map�I�y���[�V������get()��put()�́A�����ƂȂ��Ă��鏈�����������̏��������Ă���\��������܂��B����͓���̃L�[�������邽�߂Ƀn�b�V���E�o�P�b�g����������ہAget()�͑����̌���Object.equals()���Ăяo���Ȃ���΂Ȃ�Ȃ���������Ȃ����߂ł��Bkey�N���X�Ɏg�p�����hashCode()�֐����l���n�b�V���͈͂ɋψ�ɍL���Ȃ�������A�n�b�V���Փ˂����΂��΋N����ꍇ�A����o�P�b�g�̘A���͑��̃o�P�b�g��蒷���Ȃ邩������܂���B���̌��ʁA�����n�b�V���A���̌�����A��%���̗v�f��equals()�Ăяo�����x���Ȃ�\��������܂��B�����̏������ł�get()��put()�ɋN���肤����́A�A�N�Z�X���P�ɒx���Ȃ�Ƃ��������ł͂Ȃ��A�n�b�V���A������������Ă���Ԃ͑��̂��ׂẴX���b�h��Map���A�N�Z�X�ł��Ȃ��Ȃ�Ƃ������Ƃł��B

get()�̎��s�ɂ��Ȃ�̎��Ԃ��K�v�ƂȂ�P�[�X������Ƃ��������́A��Ő������������t���X���b�h�E�Z�[�t���ɂ����Č����ł��B���X�g1�Ő������������󋵂ł́A1��̃I�y���[�V�����̎��s��蒷����1�̃R���N�V���������b�N���Ă��Ȃ���΂Ȃ�܂���B�����S�ẴC�e���[�V�����̊ԃR���N�V���������b�N����Ȃ�΁A���̃X���b�h�̓R���N�V�����̃��b�N�𒷂��ԑ҂��Ă��Ȃ���΂Ȃ�Ȃ��\��������܂��B

�V���v���ȃL���b�V���̗�

�T�[�o�[�E�A�v���P�[�V�����ɂ�����Map���g�p����ł���ʓI�ȃA�v���P�[�V������1�́A�L���b�V���̎����ł��B�T�[�o�[�E�A�v���P�[�V�����̓t�@�C���R���e���c�A�������ꂽ�y�[�W�A�f�[�^�x�[�X�E�N�G���[�̌��ʁA��͂��ꂽXML�t�@�C���Ɋ֘A����DOM�c���[�A����ё��̑����̃f�[�^�^���L���b�V�����Ă��܂��B�L���b�V���̎�ȖړI�́A�T�[�r�X���Ԃ��k�����A�ȑO�̌v�Z���ʂ��Ďg�p���邱�Ƃɂ��A�X���[�v�b�g�𑝉������邱�Ƃł��B�L���b�V���E���[�N���[�h�̓T�^�I�ȓ����́A�������X�V�����͂邩�Ɉ�ʓI�ł���Ƃ������Ƃł��A���������āA�L���b�V���͑f���炵��get()�p�t�H�[�}���X��񋟂��Ă��܂��B�A�v���P�[�V�����̃p�t�H�[�}���X��ቺ������L���b�V���͍ň��ł��B

�L���b�V���̎�����synchronizedMap���g�p���邱�Ƃ́A�A�v���P�[�V�����֐��ݓI�ȃX�P�[���r���e�B�[�̃{�g���l�b�N�𓱓����邱�ƂɂȂ�܂��B����́AMap�֐V����key��value��ݒ肵�����X���b�h�����łȂ��AMap����l���������Ă���X���b�h���܂߂āA1�x�ɂP�̃X���b�h����Map�ɃA�N�Z�X���邱�Ƃ��ł��Ȃ����߂ł��B

���b�N�̗��x������������

�X���b�h�E�Z�[�t��񋟂���HashMap�̕��s�������P���邽�߂̃A�v���[�`�́A�e�[�u���S�̗p��1�̃��b�N�͂�߂āA�e�n�b�V���E�o�P�b�g�p�̃��b�N(��ʓI�ɂ́A���ꂼ��̃��b�N���������̃o�P�b�g��ی삷�郍�b�N��pool)���g�p���邱�Ƃł��B����ɂ��A�����̃X���b�h�́A�R���N�V�����S�̂ɓn��1�̃��b�N���g�p����̂ł͂Ȃ��A�����ɈقȂ�Map�ɃA�N�Z�X���邱�Ƃ��ł���悤�ɂȂ�܂��B���̃A�v���[�`���g���΁A�ȒP�ɐݒ�A�����A�폜�I�y���[�V�����̃X�P�[���r���e�B�[�����P���邱�Ƃ��ł��܂��B�������A���̕��s�������܂������Ȃ��@�\���Ȃ��ꍇ������܂��B���Ƃ��΁Asize()��isEmpty()�̂悤�ȃR���N�V�����S�̂ŋ@�\���郁�\�b�h�Ɋւ��ẮA1�x�ɑ����̃��b�N���K�v�ɂȂ�����A�s���m�Ȍ��ʂ�Ԃ����X�N�����邽�߂ɁA�������Â炭�Ȃ�̂ł��B�������A�L���b�V������������悤�ȏ󋵂ɂ͂��̃A�v���[�`�͔��ɓK���Ă��܂��B�Ȃ��Ȃ�A�L���b�V���ł͌����Ɛݒ�̃I�y���[�V�����͕p�ɂɍs���܂����Asize()��isEmpty()�͂���قǕp�ɂɍs��Ȃ����߂ł��B



	��ɖ߂�


ConcurrentHashMap

util.concurrent��ConcurrentHashMap�N���X(JDK 1.5��java.util.concurrent�p�b�P�[�W�œo�ꂵ�܂�)�́AsynchronizedMap���͂邩�ɑf���炵�����s����񋟂��Ă���Map�̃X���b�h�E�Z�[�t�ȏ����n�ł��B�����ǂݎ�肪��ɂقړ����Ɏ��s���邱�Ƃ��ł��A�����ǂݏ������ʏ�قړ����Ɏ��s���邱�Ƃ��ł��A�����������݂������̏ꍇ�����Ɏ��s���邱�Ƃ��ł���̂ł�(�֘A����ConcurrentReaderHashMap�N���X���܂��A�����悤�ȕ����ǂݎ��Ƃ������s����񋟂��Ă��܂����A�A�N�e�B�u�ȏ������݂Ɋւ��Ă͕��s�ł͂���܂���)�BConcurrentHashMap�́A�����I�y���[�V�������œK������悤�ɐ݌v����Ă��܂��B���ہAget()�I�y���[�V�����́A�ʏ탍�b�N�Ȃ��ł��܂������܂��B�������A���b�N�̂Ȃ��X���b�h�E�Z�[�t�e�B�[�ɂ͒��ӂ��K�v�ŁAJava Memory Model�̏ڍׂɂ��Ă悭�������Ă��Ȃ���΂Ȃ�܂���B�c���util.concurrent�Ɠ��l�ɁAConcurrentHashMap�̎����͐��m������уX���b�h�E�Z�[�t�e�B�[�Ɋւ��ĕ��s���̃G�L�X�p�[�g�ɍL���]������Ă��܂����BConcurrentHashMap�̎����̏ڍׂɂ��ẮA����̋L���Ō��Ă������Ƃɂ��܂��B

ConcurrentHashMap�́A�Ăяo�����Ƃ̌��ߎ��������ɂ߂邱�Ƃō������s���𓾂邱�Ƃ��ł��܂��B�����I�y���[�V�����́A���߂̐ݒ�I�y���[�V�����ɂ���Đݒ肳�ꂽ�l��Ԃ�����A�����i�s���ł���ݒ�I�y���[�V�����ɒǉ����ꂽ�l��Ԃ����肷��\��������܂�(�����Ė��Ӗ��Ȍ��ʂ�Ԃ��킯�ł͂���܂���)�BConcurrentHashMap.iterator()�ɂ���ĕԂ��ꂽ�C�e���[�^�͊e�v�f����x�ɕԂ��āAConcurrentModificationException�𓊂��邱�Ƃ͂���܂��񂪁A�C�e���[�^���\�z���ꂽ���ƂŐ������ݒ肠�邢�͍폜�Ɋւ��Ă͔��f���邩������Ȃ����A���f���Ȃ���������܂���B�R���N�V�����̌J�Ԃ��ɍۂ��āA�X���b�h�E�Z�[�t�e�B�[��񋟂��邽�߂Ƀe�[�u���S�̂����b�N����K�v�͂���܂���(�s�\�ł�������܂�)�BConcurrentHashMap�́A�X�V��h�����߂Ƀe�[�u���S�̂����b�N����K�v���Ȃ����ׂẴA�v���P�[�V������synchronizedMap�܂�Hashtable�̑���Ɏg�p���邱�Ƃ��ł��܂��B

ConcurrentHashMap�́A���L�L���b�V���̂悤�ȗl�X�Ȉ�ʓI�ȗ��p�̗L�������������ƂȂ��A��L�̕��݊��ɂ����Hashtable�����͂邩�ɗD�ꂽ�X�P�[���r���e�B�[��񋟂��邱�Ƃ��ł��܂��B

�ǂ���悢�̂��H

�\1�́AHashtable��ConcurrentHashMap�̃X�P�[���r���e�B�[�̈Ⴂ�ɂ��Ă̑�܂��Ȍ����ł��B���ꂼ��̎��s�ɂ����āAn�X���b�h��Hashtable�܂���ConcurrentHashMap�Ń����_���ȃL�[�l����������悤�ȃ^�C�g�ȃ��[�v�𓯎��Ɏ��s���܂����B�������ʂ�put()�I�y���[�V�����̎��s�ł�80%���s���Aremove()�I�y���[�V�����̎��s��1%�������܂����B�e�X�g�́ALinux���N�����Ă���f���A���v���Z�b�T�[Xeon�V�X�e���ōs�Ȃ��܂����B�f�[�^��ConcurrentHashMap��1�X���b�h�Ő��K������āA10,000,000��J�Ԃ������s���Ԃ��~���b�Ŏ����Ă��܂��BConcurrentHashMap�̃p�t�H�[�}���X�̓X���b�h�������Ȃ��Ă��X�P�[���u���Ȃ܂܂ł���̂ɑ΂��āAHashtable�̃p�t�H�[�}���X�̓��b�N�������������āA�����ɒl���������Ă��邱�Ƃ���������ɂȂ�ł��傤�B

���̃e�X�g�ł̃X���b�h���͑�\�I�ȃT�[�o�[�E�A�v���P�[�V�����Ɣ�r���āA���Ȃ������邩������܂���B�������A���ꂼ��̃X���b�h�̓e�[�u�����J��Ԃ� hit���Ă���̂ŁA���ۂɎ኱�̃R���e�L�X�g���e�[�u�����g�p����X���b�h�����͂邩�ɑ����̋������V�~�����[�g���Ă��܂��B

�\1. Hashtable��ConcurrentHashMap�̃X�P�[���r���e�B�[
Threads 	ConcurrentHashMap 	Hashtable
1	1.00	1.03
2	2.59	32.40
4	5.58	78.23
8	13.21	163.48
16	27.58	341.21
32	57.27	778.41



	��ɖ߂�


CopyOnWriteArrayList

CopyOnWriteArrayList�N���X�́A�}����폜�������|�I�Ɍ������������s�A�v���P�[�V������ArrayList�ɑ�����̂Ƃ��Ă݂Ȃ���܂��BArrayList���AAWT��Swing�A�v���P�[�V�������邢�͈�ʓI��JavaBean�N���X�̂悤�ɁA���X�i�[�̃��X�g���i�[���邽�߂Ɏg�p�����ꍇ�́ACopyOnWriteArrayList�ƑS�������ł��B(�֘A����CopyOnWriteArraySet�́ASet�C���^�t�F�[�X���������邽�߂�CopyOnWriteArrayList���g�p���Ă��܂��B)

���X�g���ς̏�ԂŁA�����̃X���b�h�ɃA�N�Z�X�����\��������ɂ��ւ�炸�A���X�i�[�̃��X�g�̊i�[�ɒʏ��ArrayList���g�p����Ȃ�΁A�J�Ԃ��̊ԑS���X�g�����b�N���邩�A�J�Ԃ��̑O�Ƀ��X�g�̃N���[�������Ȃ���΂Ȃ�܂���B�����͂���������Ȃ��Ԃ̂����邱�Ƃł��BCopyOnWriteArrayList�͂��̑��ɁA�ύX�̃I�y���[�V�������s�Ȃ��ꍇ�͏�Ƀ��X�g�̐V�K�R�s�[���쐬���܂��B�܂��ACopyOnWriteArrayList�̃C�e���[�^�́A�C�e���[�^���\�z���ꂽ���_�ŕK�����X�g�̏�Ԃ�Ԃ��āAConcurrentModificationException�𓊂��܂���B�C�e���[�^�����郊�X�g�̃R�s�[�͕ς��Ȃ��̂ŁA�J�Ԃ��̑O�Ƀ��X�g�̃N���[�����������A�J�Ԃ��̊ԃ��b�N����K�v�͂���܂���B����������΁ACopyOnWriteArrayList�́A�s�ϔz��ւ̉ς̎Q�Ƃ��܂�ł��܂��B���������āA���̎Q�Ƃ��Œ肳��Ă������A���b�N�̕K�v�Ȃ��s�ς̃X���b�h�E�Z�[�t�̗��_�𓾂邱�Ƃ��ł��܂��B



	��ɖ߂�


�v��

synchronized collections�N���X�AHashtable�AVector�A����ѓ��������b�p�[�E�N���X��Collections.synchronizedMap��Collections.synchronizedList�́AMap��List�̊�{�I�ȏ����t���X���b�h�E�Z�[�t�̎�����񋟂��Ă��܂��B�������A�����̎g�p�͎��̂悤�ȗv���ɂ��A���x�ȕ��s�A�v���P�[�V�����ɂ͓K���Ă��܂���B�P�̓R���N�V�����S�̂ɓn��1�̃��b�N�̓X�P�[���r���e�B�[�̏�Q�ł��邱�ƁA2�ڂ͌J�Ԃ��̊�ConcurrentModificationExceptions��������邽�߂ɂ́A�����Ȏ��ԃR���N�V���������b�N����K�v������A�Ƃ����R�ł��B�������AConcurrentHashMap�����CopyOnWriteArrayList�̎����ł́A�Ăяo�������኱�̕��݊������邱�ƂŃX���b�h�E�Z�[�t���ێ����������s����񋟂��邱�Ƃ��ł��܂��BConcurrentHashMap�����CopyOnWriteArrayList�́AHashMap�܂���ArrayList���g�p���Ă����S�Ăɂ����ĕK�������𗧂Ƃ͌���܂��񂪁A�����common�󋵂��œK������悤�ɐ݌v����Ă��܂��B�����̕��s�A�v���P�[�V������ConcurrentHashMap�����CopyOnWriteArrayList���g�p���邱�ƂŁA���v�𓾂���悤�ɂȂ�̂ł��B


�Q�l����

    * Brian Goetz���ɂ�� Java�̗��_�Ǝ��H �̂��ׂĂ̋L�������ǂ݂��������B���ɍ���̋L���Ɗ֌W������̂́A�u�ϐ����H�s�ϐ����H�v(developerworks �A2003�N2��)�ŁA�s�ϐ��̃X���b�h�E�Z�[�t�e�B�[�̗��_�Ɋւ��Đ�������Ă��܂��B

    * Doug Lea���́wConcurrent Programming in Java, Second Edition �x�́AJava�A�v���P�[�V�����ł̃}���`�X���b�h���v���O���~���O�Ɋւ�������Ȗ��ɂ��ď����ꂽ���΂炵���{�ł��B

    * util.concurrent �p�b�P�[�W���_�E�����[�h���Ă��������B

    * javadoc��ConcurrentHashMap�Ɋւ���y�[�W�ł́A ConcurrentHashMap��Hashtable�̈Ⴂ�ɂ��ďڂ�����������Ă��܂��B

    * JSR 166 �́AJDK 1.5�p��util.concurrent ���C�u�����[��W�������Ă��܂��B

    * 2002�N11����Java�̗��_�Ǝ��H �̃R�����ł́Autil.concurrent�̎��s�@�\����舵���Ă��܂��B

    * �����R�X�g�̍팸�ɂ��ẮA�u�V�X�e�����ׂ��y�������X���b�h��: ������ጸ������v(developerworks �A2001�N9��)�ŉ������Ă��܂��B

    * ���̑���Java�Q�l�����Ɋւ��Ă�developerWorksJava technology �]�[�� ���Q�Ƃ��Ă��������B



���҂ɂ���

		

Brian Goetz��18�N�Ԉȏ�ɓn���āA���I�\�t�g�E�F�A�J���҂Ƃ��ē����Ă��܂��B�ނ̓J���t�H���j�A�B���X�A���g�X�ɂ���\�t�g�E�F�A�J���R���T���e�B���O��ЁAQuiotix�̎�ȃR���T���^���g�ł���A�܂���������JCP Expert Group�̈���ł�����܂��B2005�N�̖��ɂ�Addison-Wesley����ABrian�ɂ�钘�AJava Concurrency In Practice���o�ł����\��ł��BBrian���ɂ��L�͋ƊE���Ɍf�ڍς݂���ьf�ڗ\��̋L���̃��X�g���Q�Ƃ��Ă��������B 
